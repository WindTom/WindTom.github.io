---
layout: post
title: "论文分析"
description: ""
category: 论文分析
tags: [存储,ext4]
---
* content
{:toc}

摘要：Drive Managed SMR disk较传统磁盘，提供了可插拔高容量替代方案。这种方案在非序列化工作任务下的表现出双峰行为：经过短期高吞吐后进入持续低吞吐期。





我们提出ext4-lazy方法，对linux的ext4文件系统进行微小改动，就能显著提高以上两种模式下的吞吐量。在两家厂商的四种不同drive-managed SMR磁盘上验证结果证明，ext4-lazy较ext4，在metadata-light file server标准上的性能能够提高1.7-5.4倍；在metadata-heavy标准上不管是drive-managed SMR磁盘还是传统磁盘，性能都快2-13倍。

## 1.介绍

世界上超过90%的数据是过去两年产生的。为跟上数据快速增长的步伐，同时为了增强与SSD的竞争力，硬盘厂商发力研究提高容量的技术，像Shingled magnetic recording、HAMR，和BIT-Patterned magnetic recording. HAMR和BPMR仍在研究阶段，SMR能够让硬盘制造商在现有fabrication方法上提高areal  density。不过，这是以增加复杂度为代价的，导致硬盘与CMR硬盘的行为不同。而且，既然SMR能够作为HAMR和BPMR的补充，在areal   desity上提供更大增长，未来可能所有的高容量硬盘都使用SMR。

工业界曾尝试过两种SMR方案：drive-managed SMR，和host-managed SMR（HM-SMR）。DM-SMR磁盘是传统磁盘的仓促替代品，在传统块接口上提供更高容量，但在非序列化写时性能大幅下降。DM-SMR不像CMR一样在随机写时保持低但稳定的吞吐量，在短期高吞吐后随即陷入低吞吐量（图1）。

相反，HM-SMR提供backward-incompatible接口，这个接口需要对I/O stacks做较大改动以便让能够感知SMR的软件优化自身访问模式。

新的HM-SMR接口让存储研究者们在其上提出了新的文件系统，但有意思的新问题也随之产生。同时，这也给长期在CMR磁盘上进行编码优化工作的开发者带来了挑战。为了使用新接口，有人尝试修改已经成熟的Linux文件系统，如ext-4和XFS，但由于重新设计工作量巨大，这些尝试进展缓慢。虽然Log-structured file system的结构是最容易适配HM-SMR接口的，但基于它的磁盘文件系统在实际中并没有达到生产质量要求。

为了使用SMR，我们采取了替代方案。不是针对HM-SMR接口进行重新设计文件系统，而是对成熟的、高性能的文件系统做增量改变，优化其在DM-SMR磁盘上的性能。我们首先为DM-SMR磁盘优化ext4文件系统，发现随机写在这些磁盘上代价更加昂贵，其中metadata writeback是最主要的问题来源。

我们提出ext4-lazy，对ext4文件系统稍加改动便减少了大部分的metadata writeback。同其他的日志文件系统一样，ext4写metadata两次：如图2所示，它先将metadata block写到系统磁盘的一个临时位置J，然后在内存中将该block标记为dirty。一旦在内存中待的时间达到一定阈值，writeback线程（或者flusher）将把该block写到硬盘的固定位置S上，导致随机写。尽管metadata writeback是workload中很小的一部分，它导致了很多随机写，如图3。相反，ext4-lazy在将块写入日志系统后将其标记为clean，以阻止writeback,同时在内存中的map里插入一个(S,J)对，以便文件系统能够访问这个block（图2b）。  ext4-lazy使用一个大的journal，以便其能继续写blocks同时回收旧blocks。在mount过程中，它从日志中重建内存里的map，导致mount时间稍有增加。我们的实验证明，metadata-heavy的workload中，ext4-lazy显著提高DM-SMR的性能，在CMR硬盘上也是如此。

我们在这边文章中的主要共享是在DM-SMR和CMR磁盘上设计、实现和评估了ext4-lazy。我们的改动非常小：我们改动了80行现有代码，同时添加了一个600行的C文件。在metadata-light（<=1%的写）情况下，ext4-lazy提高DM-SMR磁盘吞吐量达1.7到5.4倍。在directory traversal和metadata-heavy 的workloads的情况下将DM-SMR和CMR磁盘的性能都提高2-13倍。

此外，我们还做出两点贡献：
---对于纯sequential write workloads，DM-SMR硬盘能达到全负荷吞吐量，性能不会降低。我们发现最小的sequential I/O size触发DM-SMR的这种行为。
---我们证明，对于physical journaling，小journal是metadata-heavy workloads的瓶颈。基于我们的实验结果，对于超过128G的文件系统，ext4开发者已经将默认的journal size从128M提高到1G。

在接下来的章节中，我们将给出SMR的背景，指出为什么随机写在DM-SMR磁盘上性能差，为什么metadata writeback在ext4中导致更多随机写（第2节）。接下来，我们介绍ext4-lazy，给出设计和实现（3节）。最后评价实现（4节），介绍相关工作（5节），总结（6节）。源代码和其他资源请访问http://www.pdl.cmu.edu/Publications/
downloads.shtml.

## 2 背景
我们介绍SMR技术，它是如何工作的，然后描述ext4怎么在磁盘上存放数据，它如何使用kernel的layer完成journaling的工作的。

### 2.1 DM-SMR内部原理


如同SSD磁盘有Flash Translation Layer（FTL）一样，DM-SMR磁盘也有一个Shingle Translatin Layer(STL)。STL管理硬件中的数据，对外则提供block接口。
> 备注：FTL是SSD上的一个软件层，是最为核心和重要的技术，也是竞争力的保证。

目前，所有STL都提供一个甚至多个persistent cache，以接纳随机写，避免每次写时都出现RMW（read-modify-write）。因此，当一个写操作更新某个band的一部分时，STL将更新写入persistent cahce，同时该band变为dirty。STL负责在空闲时清理dirty band，清理的方法是将cache中的更新数据同band里没有改变的数据合并，然后写入band，然后释放cache里更新数据所占用的空间。

根据使用的block mapping类型的不同，清理一个band的成本也不同。dynamic mapping方法里，STL读取band，在内存中将其更新，然后写入一个新的不同的band中，并修改映射。这导致一次读和一次写。static mapping则需要先将更新后的band写入到一个临时空间（scratch space），因为直接在原band上写会导致断电故障时数据丢失。这导致一次读和两次写。

图4d展示Seagate ST8000AS0002 DM-SMR磁盘的逻辑结构。该磁盘大约有26万个band，每个band的大小在30MiB；sectors映射到固定band上；另外还有大约25GiB的persistent cache（对host不可见）。STL检测到顺序写后开始将它们顺序写入band，而不经过persistent cache。但是，当检测到随机写时，就将数据写入persitent cache，并将band置为dirty。清理一个band大概耗费1-2秒，极端情况下大概耗费45秒。

不同STL的清理策略也不同。一些STL持续小量清理，另外一些空闲时清理。如果数据还没写完，但是cache被填满了，STL就会强制交错执行清理工作和写入工作。

图1展示了两家厂商的DM-SMR磁盘性能。希捷磁盘空闲时清理，且是static mapping（一读两写），因此persistent cache没满时，他们的吞吐量非常高，但满了就超低。图中，不同型号的希捷磁盘吞吐量下降的时间点也不一样，这是因为不同型号磁盘的persistent cache也不一样。西数磁盘则持续地清理，且是dynamic mapping（一读一写），因此当persistent cache没满时吞吐量较希捷小，但cache满后又较希捷高。

###2.2 ext4和日志（journaling）

ext4文件系统从ext2进化而来，它受Fast File System(FFS)影响较大。同FFS一样，exit2将磁盘划分成柱面组，或者用ext2的叫法——block groups。为了进一步增强局部性，block groups内用来代表文件的metadata blocks(inode bitmap,block bitmap,inode table)也被保存在该组内且位于组的头部，如同图5a所示。group descriptor block在block group里的位置是固定的，因此通过它就可以找到metadata blocks。

在ext2中，block groups的大小被限制在128MiB,4KiB位能够表示的最大数量。ext4进入了flexible block groups（flex_bgs）的概念，如图5b所示，flex-bgs包含一组block group，位于前16MiB的是整个flex-bgs的元数据。

ext4保证journaling时metadata的一致性，但它并不自己实现journaling，而是使用称作journaling block device的通用kenel层。journaling block device运行在一个称作jbd2的kernel线程里。当发生文件系统的操作时，ext4从磁盘读取metadata blocks并在内存中更新，并开放给jbd2进行journaling。为了提高性能，jdb2将一段时间内发生的文件系统操作所导致的元数据更新作为一批（默认5秒一批），统一commit到一个transaction buffer，并原子性地将该transaction导入journal————一个带有头尾指针的transactions的circular log。当该buffer满时，或者接到同步写请求时，transaction可能被提前导入。

值得注意的是，一个transaction除了包含metadata blocks，还包括descriptor blocks（记录metadata blocks在该transaction中的static position）。在一次commit后，jbd2将metadata blocks在内存中的拷贝置为dirty，以便writeback线程将其写入他们的static positions。如果一个in-memory metadata block在diry timer到期前发生更新，jdb2将该block作为新的transaction写入日志，并重设该block的timer，延迟writeback。

在DM-SMR磁盘上，当metadata blocks被writeback时，他们将弄脏flex_bg上所对应的bands，如图5c。一个metadata与band并不对齐（可能在1个band上，也可能跨2个），因此metadata的写入可能弄脏0个、1个或2个band。


## 3 ext4-lazy的设计和实现
首先从整体上介绍设计，而后详细实现

### 3.1 动机

ext4-lazy的动机来自两个观察：
（1）ext4的metadata writeback造成的随机写，导致DM-SMR磁盘巨大的清理负担。
（2）文件系统的metadata占一小部分blocks，且hot metadata（被频繁更新）占比更小。
从后一个观察得出，在比hot metadata大数倍的circluar log里管理hot matadata能将随机写转成纯序列化写，从而减少DM-SMR磁盘的清理负担。

我们首先给出支持第一个观察的计算数据，然后给出第二个观察的实验证据。

一个8T的partition，大约有4000个flex_bg。每个flex_bg的前16Mib是metadata。如图5C所示。按一个band 30MiB算，更新每个flex_bg平均会污染4000个band，也就是需要清理120G的band，产生360G的磁盘流量（一读两写的情况下）。一个涉及磁盘1/16容量的worload，也就是500G的文件，会弄脏至少250个band，需要22.5G的清理工作。如果考虑到像extent tree blocks和directory blocks这样的floating metadata，清理工作会更加繁重。

为了测量hot metadata的比例，我们在ext4上模拟一个build server的I/O负载。实验运行128个并行的Compilebench实例，并将磁盘完成的所有写任务进行归类。在433G的写任务中，388G是写数据，34G是写journal，11G是写metadata。unique metadata blocks的总大小是3.5G，仅占0.8%的总写任务量，且90%的journal writes是覆盖写。

### 3.2 设计
在高层，ext4-lazy在ext4和jbd2中添加以下组件：
Map：ext4-lazy用jmap跟踪metadata blocks在journal中的位置，也就是内存中有一个map,将一个metadata block的static位置S与其在journal中的位置J联系起来。每次当有一个metadata被写入journal的时候（图2b），map都会更新。

Indirection：在ext4-lazy，所有对metadata blocks的访问都会经过jmap。如果最新版本的block在Journal里，jmap会有一个entry指向它；如果发现没有entry，那么位于static位置上的就是最新的block。

Cleaner：ext4-lazy中的cleaner释放变成stale的journal的空间（同一metadata block写入了新的拷贝，因此该位置处的数据过时）

Map recontruction on mount：每次Mount,ext4-lazy从位于journal的tail和head指针中间的transactions中读取descriptor blocks，并更新jmap。

### 3.3 实现
我们在jbd2中，用红黑树实现jmap。当jbd2 commit一个事务后，它更新jmap（更改本次事务所涉及的metadata blcoks的map），并将这些block在内存中的拷贝标记为clean，以防止它们被writeback。我们通过更改读取metadata blocks的call sites，在ext4中添加indirection查询metadata blocks的函数（jmap中查询metadata block位置的函数），如listing 1。ext4的代码修改量在40行。

indirection允许ext4-lazy后向兼容，逐渐将metadata blocks移动到journal。不过indirection的主要原因是在清理的过程中将冷metadata移动到其static位置，在journal中保留hot metadata。

我们利用现有功能在jdb2中实现cleaner只用了400行C代码。尤其是，cleaner只从journal的tail中读取live metadata blocks，并通过ext4使用的接口将它们加入到transaction buffer。对于每个transaction，它保留一个doubly-linked List，指向jmap中live blocks 。更新一个jmap entry就作废一个block并将其从对应的List中去除。清理事务时，cleaner在恒定时间内通过该事务的list找到live blocks，读取并加载到transaction buffer。这个cleaner的美丽之处在于它不会中断文件系统的正常运行，如同并行的操作一样。目前我们使用了一个简单的清理策略，将来可能开发更加优秀的策略（比如冷热分离）。

map reconstruction只改动了jbd2的一小部分恢复代码（recovery code）。stock ext4在关机时重置。在mount上发现非空journal代表机器crash了，并触发recovery process。在ext4-lazy，journal的状态是jmap的永久镜像，因此ext4-lazy从不重置journal并一直“recover”。在我们的原型中，ext-lazy通过读取journal tail和head指针之间的事务的descriptor block（描述性块）来重建jmap。head和tail指针之间的空间约等于1GiB的时候，耗费时间5-6秒。

## 4 评估

实验采用的设备为：四核i7-3820(sandy bridge)3.6Ghz CPU，16GRAM，系统为Ubuntu 14.04，内核Linux kernel 4.6。使用的磁盘见Table 1中所列。为降低多次启动带来的误差，我们unmount文件系统，启动的时候总是使用同一种文件系统状态，格式化ext4分区(partition)时关闭lazy initialization，将磁盘writeback缓存比例固定为50%（默认情况下，这个比例是通过writeback throughput动态计算而得）。每个实验都重复做至少5次，并给出均方差。

### 4.1 Journal 瓶颈
实验发现对于metadata-heavy workloads，ext4默认的128MiB journal是一个瓶颈。之所以提这件事是因为它影响我们选择baseline。Table 1中列出的CMR磁盘WD5000YS的瓶颈验证方法是使用Filebench的CreateFiles microbenchmark在超过6万个文件夹中创建10万个小文件。workload size约等于1GiB，且fits in memory。

尽管ext4-lazy使用一个大journal，因为在ext4上启用一个大journal是命令行，我们选择10GiB journal的ext4作为baseline。本文剩下部分中将默认journal size为128M的ext4称为ext4-stock，10G journal的ext4为ext4-baseline。

我们测量ext4在内存中创建文件的速度时并不考虑writeback时间。图6a展示了，在ext4-stock，benchmark完成用时约460秒，在ext4-baseline上快46倍，约10秒。接下来，我们证明小journal是如何称为瓶颈的。

ext4 journal是事物的循环日志，有一个head和tail指针。当文件系统进行操作时，jbd2向journal发送事务，前移head指针




