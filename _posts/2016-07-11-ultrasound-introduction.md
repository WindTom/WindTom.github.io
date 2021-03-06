---
layout: post
title: "超声、超声心动图和多普勒基础入门（一）"
description: ""
category: ultrasound
tags: [ultrasound]
---



----------
* content  
{:toc}  

英文原文链接: 
（http://folk.ntnu.no/stoylen/strainrate/Basic_ultrasound#ultrasound） 

本文的目的是尽量以图片介绍超声心动图和多普勒基础知识。

题外话：看老外的科普性的文章，最大的感受就是直观，很容易让人明白，而国人总喜欢取术语，而且取名又让人很难理解其中的含义。百度上搜一个问题，不同的博客给的都是同一个答案。如果我们都能少一些拷贝粘贴，多写一些理解性的话，那别人学起来就容易多了。





###  超声
简单来说，超声就是声波，和人的声音一样，只是你听不见而已。声音是纵波，沿声音传播方向前后振荡。 
人能听见的声音大概在15000到20000赫兹，而临床上超声的频率在1000到12000赫兹。我们能墙角拐弯处能听见声音，是因为声音在这些弯角处发生衍射；高频（短波长）声音则会像电磁波一样沿直线传播，遇到物体时反射回来的波束能量也更为集中，就像光束的反射一样。因为波长短，高频声波更易在较小的物体上发生发射，且在气态媒介中不易传播。 
高中物理课本告诉我们波长和频率f成反比，它们与速度v的关系是:


<img src="https://github.com/WindTom/imagestom/blob/master/gongshi.gif?raw=true"><br>

声波在不同媒介中的速度不同。

脉冲电压加在晶片上，晶体产生振动，从而产生超声波，我们叫这种晶体为压电晶体。同一个晶体可以作为反射超声波的接收者。

## 什么是超声数据？

超声数据按照复杂度可作如下分类：
<div>
<table style="text-align: center; width: 100%;" border="1" cellpadding="2" cellspacing="2"> 
   
<tr >
<th width="33%" ><img  src="https://github.com/WindTom/imagestom/blob/master/RF.GIF?raw=true" ></th>

<th width="33%"><img  src="https://github.com/WindTom/imagestom/blob/master/RF-ampl.GIF?raw=true" width="45%" >        <img  src="https://github.com/WindTom/imagestom/blob/master/ampl.GIF?raw=true" width="45%" ></th>

<th width="33%"><img  src="https://github.com/WindTom/imagestom/blob/master/freq.GIF?raw=true" > </th></tr>

<tr>
<td><span><small><b>通常，接收到的反射超声回波是一个波形（也称RF Data），但是存储这种波形对存储空间要求很大，因为波形曲线上每个点都要求能够表达出来。不过，如果能够将RF Data存储下来，回波的振幅和频率也能借此计算出来。</b></small></span> 
</td>
<td ><small><span><b>超声回波具有振幅，只存储振幅的开销要少得多。这也是灰度成像中使用的唯一数据，比如B型和M型超声中各点（散射体对应着各个点）的亮度就是振幅信息。</b></span></small>			   
</td>
<td ><small><span><b>同样，超声回波也具有频率信息，这些信息同样可以作为像素值，在图像上显示出来。比如多普勒成像。另外，这种数据对存储空间的要求比RF Data少很多。</b></span></small> 
</td>
</tr>
      
</table>
</div>
<br>
   
## 超声成像

### 反射和散射

一般来说，所有超声成像的原理都是，发射出去的超声波在经过两个组织的边界时，部分能量被反射回来。如下图三条竖线代表三个边界。反射能量的多少根据各组织的阻抗而定。通常见到的超声成像只用到了振幅信息。

当一束超声波被发射出去，接收者每隔一小段时间就接收一下（取样）。因为超声的速度是恒定的，从超声发射出去到接收到反射回来信号的时间只与距离有关，也就是说与该组织的距离有关。这个距离对应到图像上就是深度信息。不同的组织结构反射回来的能量也不同，所以反射回来的信号携带两种信息：深度信息和振幅信息。下一条超声波什么时候发射取决于想探查的最大深度。

用下面图来解释，P点投射出去一支超声波大军去撞墙（组织边界），有的小兵比较猛，直接穿墙，有的小兵头破血流地回来了（超声波被反射回来）。与此同时一个接收者在P点就等着统计返回来的小兵数量。
<div align="center">
<table style="text-align: center; width: 100%;" border="1" cellpadding="1" cellspacing="1" >

<tr>
<td width="50%"><img src="https://github.com/WindTom/imagestom/blob/master/Amode.gif?raw=true"></td>
<td width="50%"><img src="https://github.com/WindTom/imagestom/blob/master/Amode1.JPG?raw=true"></td>
</tr> 
</table> 
</div>
 <br>

过了一秒，抬头检查，没人来，先休息下；又过了一秒，发现回来了几个小兵，这肯定是撞第一个墙的人来了。数一下人数和时间，休息去。

过了一秒，抬头检查，没人来，难道剩下的小兵都很猛吗?休息去 
又过了一秒，抬头检查，没人来，这么猛？！休息去 
再过了一秒，还是没人来，这批人吃补药了？休息去。 
又过了一秒，抬头检查，回来一批。原来不是补药吃的好，是距离有点远耶。统计一下回来的人数，记录时间，休息去。

就这样每隔一秒抬头检查一次，过了一段时间，又有一批小兵回来了，这应该是撞到第三面墙了，继续统计。继续……

这里，统计员记录的时间就是深度信息（距离信息），统计的小兵人数就是对应的振幅信息。

我们可以计算到，发射源与反射物体间的距离为声波速度与统计时间的乘积再除以2，为什么除以2？（因为小兵走了一个来回，与目的地之间的距离当然是一半了）

某一时刻也就是某一深度接收到的能量值，以能量振幅的形式显示就是A型超声。以发射矩阵上某点的亮度值显示就是B型超声。如果有一些散射体在动，则可以让B型超声的点在屏幕上动起来，这就叫M型超声。

可以参见下图，下图中离P点近的是第一个边界（散射体），最远的是第三个散射体。且第三个散射体在运动。A型超声反映每个散射体的深度和反射能量信息。B型超声以亮度表示反射回来的能量大小。最下面一个散射体在动，如果用时间曲线来反映，第三个反射体的运动就是一条曲线（M型超声） 
<div align="center">
<table style="text-align: center; width: 100%;" border="1" cellpadding="1" cellspacing="1">
<tr>
<td><img src="https://github.com/WindTom/imagestom/blob/master/sektor_puls1.gif?raw=true"></td>
<td><img src="https://github.com/WindTom/imagestom/blob/master/abm.gif?raw=true"></td>
</tr>
</table> 
</div> <br>

脉冲入射能量和反射能量的比例被称为反射系数（reflection efficient）；入射能量和透射能量的比例被称为透射系数（transmission coefficient）。这两个系数都取决于界面两侧材料的声阻抗。一种传输媒介的声阻抗定义为声音在该媒介中的传播速度c和该媒介的密度之积：公式1    

$$ z=c\times \rho $$

因此，如果声音在两种媒介中的传播速度相差悬殊，则声音在其中间界面上的反射率接近100%，也就是说能量无法从第一个媒介传播到第二个媒介。这种情况常发生于软组织和骨头之间的界面，或者是软组织和空气之间的界面上。这时可以认为第二个媒介存在于阴影中。

声波被以多个方向反射，反射波并不能直接返回发射器，因此这种反射结构也常被称为**散射体**（scatterers）。

**需要注意的是反射回探头的能量（也就是反射信号振幅）的多少，不只取决于反射系数，反射信号的方向同样重要。**  
因此：  
-无规律的，或者说不均匀的散射体只能将部分能量反射回探头
-在反射面垂直于超声波入射方向时，散射体越匀质，反射的能量越多。
<div align="center">
<table  border="1px" width="80%">
<tr>
<td><img src="https://github.com/WindTom/imagestom/blob/master/Scatterers.GIF?raw=true" width="100%"></td>
</tr>

<tr>
<td><p><small><b>反射面大小和方向对反射能量的影响。左侧两图为平面反射面，当平面与入射波垂直，绝大部分能量将被反射回探头（这里并不是全部能量，因为入射波束前锋并不是平的）；当平面倾斜45度，几乎绝大部分能量将被反射到其他方向，探头接收到的反射能量微乎其微。中间两图展示了弯曲的散射面，这时会有更多能量被反射到不同方向，当然探头接收到的能量相对更少，但如果将散射面倾斜，可能会有更多能量反射到探头。比如，当心脏室壁收缩时，室壁方向就会发生变化。最后一张图展示了一个完全不规则的散射面，入射能量被反射到各个方向，但探头接收到的能量非常少。</b></small>
</p>
</td>
</tr>
</table>
<br>

<table border="1px" width="100%" align="center">
<tr>
<td width="50%"><img src="https://github.com/WindTom/imagestom/blob/master/reflected%20direction.JPG?raw=true" width="100%"></td>		
<td width="50%"><img src="https://github.com/WindTom/imagestom/blob/master/Tissuechar.jpg?raw=true" width="100%"></td>	
</tr>	

<tr>		
<td><p><small><b>上图为一幅左心室长轴图，反映了反射面方向不同所造成的影响。三处箭头所指处被称为隔血层（septum-blood interface），淡绿色箭头所指处由于表面垂直于超声波入射方向，其亮度要高于表面倾斜于入射波方向的区域（红色箭头）。</b></small></p></td>
<td><p><small><b>上图为心动周期内整体散射(integrated backscatter)振幅的周期性变化。这不是对心肌密度变化的反映，因为心肌是不能压缩的，所以振幅的变化大多数反映了心肌纤维的方向变化。</b></small></p></td>
</tr>
</table>
<br>
</div> 

注意：术语**反射**指的是反射回探头的信号，而**散射**(scattering)指的是各个方向上的反射信号。

因此，超声图像上显示的组织密度取决于室壁和纤维方向。心脏某个部位的纤维方向与入射超声波垂直时，从图像上看密度较大。而实际上振幅（被反射信号的明暗）并不足以反映组织密度，也有可能是反射方向不同。因此，整体散射可以用来研究周期性变化，但不能用于研究组织特性。


（下页继续）



