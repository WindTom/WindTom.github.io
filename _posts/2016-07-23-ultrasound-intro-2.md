---
layout: post
title: "超声、超声心动图和多普勒基础入门（二）"
description: ""
category: ultrasound
tags: [ultrasound,超声]
---
* content
{:toc}

英文原文链接：   
（http://folk.ntnu.no/stoylen/strainrate/Basic_ultrasound#ultrasound） 

## **吸收**

入射超声波的一部分被组织吸收，转化成热量。这表示被吸收的能量过高可能导致生物反应。




细胞吸收超声波的问题必须受到重视，原因有二：

1. 细胞被加热程度决定了超声设备的安全标准。被吸收的能量应该被限制在不足以破坏组织细胞。如今，细胞吸收率已经能够被计算并且应用于商业医疗设备的标准之中（被称为机械指数（mechanical index））。

2. 超声波能量衰减。细胞吸收是超声波束穿透能力（穿透深度）的主要障碍。

决定能量吸收能力的因素有两个：

1. 组织密度。密度越高，吸收越多。所以超声波在媒介中的衰减率为液体<脂肪<肌肉<纤维组织<钙化和骨头

2. 超声波束频率。频率越高，吸收越高。超声在人体组织内衰减情况大概为 1 db/cm MHz。（不过，这是单向的衰减数据，实际中要考虑到超声波要走一个来回才能成像）。因此，探查的深度确定后，就可以决定超声频率上限。虽然加大发射的超声波能量也能提高穿透深度，但这无疑会增加被吸收的能量，而组织吸收能量是有限制的。

## 超声功率/机械指数

超声功率指的是探头发射能量的振幅，也就是发射进病人身体内的总能量，单位是分贝。

机械指数指的是病人吸收的能量。这不仅取决于超声功率，还取决于波束对焦点（波束对焦的区域最高），但随着深度的增加而减弱。机械指数是衡量超声波生物反应的一个指标，医疗设备上标注的一般是最大值。通常，机械指数在1.5（B型超声）到0.1之间。

## 衰减

超声波在传输过程中由于反射和散射会产生衰减。因此可以想象，为了成像就必须反射回部分超声波，而这会导致衰减。

基本上，高反射率的物体会导致图像上产生衰减阴影（如下图）。位于低密度（反射率）器官背后的组织会更明亮。
<div>
<table style="text-align: center; width: 100%;" border="1" cellpadding="1" cellspacing="1" >

<tr >
<th width="50%"><img src="https://github.com/WindTom/imagestom/blob/master/attenuation.GIF?raw=true"></th>
<th width="50%"><img src="https://github.com/WindTom/imagestom/blob/master/Ultrasound%20gallstones.JPG?raw=true"></th>
</tr>

<tr >
<td width="50%"><p>	<small><b>上图为位于不同反射率结构后面的同一个匀质组织（比如肝脏）的成像示意图。高反射率细胞组织（如钙化组织）的背后会产生高衰减（图上左侧白色圆圈），因此其后面的扇区看起来明显更暗，甚至可能完全是阴影。低反射率组织结构（如液体）的背后衰减就很轻（右侧黑色圆圈），其后面的组织能接受到更多的超声波，在成像上与周围组织相比看起来更密（明亮）。</b></small></p>
</td>
<td width="50%"><p>	<small><b>上图为肝脏组织的成像，其前面有胆囊，胆囊中又含有高密度的结石，造成结石后方出现阴影。胆囊中的其余地方充满液体，因此位于这些部位后方的扇区密度更大，看起来也更明亮或称coloring</b></small></p></td>
</tr>
</table>
</div>
<br>
衰减大约造成10%的能量损失，加上超声波的衍射，能够被反射回来成像的能量就更少。不过最主要的原因是细胞吸收。另外，衰减随着深度的增加也加重，再加上回程的能量损失，反射回探头的能量就更少了。

衰减是限制超声波束穿透深度（这个深度指的是超声波发射出去后，探头仍能接收到有效信号的距离）的主要因素。波长越短，衰减越大，穿透距离也越短。

有效的距离大概在200-300x（这个单位在原文中也看不清）。临床上，质量好的成像的穿透参数大约为3.5MHz 10~20cm（成人心脏探查量），5MHz 5~10cm（儿科心脏探查），7.5 MHz 2~5cm, 10 MHZ 1~4 cm。后两个频率应用在血管探查。不过，避开衰减的途径之一是谐波成像（harmonic imaging）。也就是说，以一定频率发射出去的波束，探头接收到信号后以两倍频率进行分析（傅里叶分析），这提高了反射信号的信噪比，而不会有分辨率损失，尤其是在图像的最深处。（*什么是最深处？指图像上远离发射源的位置*）

## 增益

增益（gain）可以对抗衰减，方法是在后处理中对反射信号进行增益补偿。不过加大增益补偿的同时也提高了信噪比（**疑问：这是好事还是坏事？**）。增益补偿可以在获得图像的同时进行，也可以在后期处理中进行。

<div align="center">
<table style="text-align: center; width: 100%;" border="1" cellpadding="1" cellspacing="1">

<tr>
<td width="50%"><img src="https://github.com/WindTom/imagestom/blob/master/attenuation.jpg?raw=true">
</td>
<td width="50%"><img src="https://github.com/WindTom/imagestom/blob/master/attenuation%20gain.jpg?raw=true">
</td>
</tr>

<tr>
<td><p><b><small>未补偿的图像。可看到信号强度（可见度）随着深度不断衰减</small></b></p>
</td>
<td><p><b><small>增益补偿的图像。加大了信号的振幅，使得扇区后端组织结构更为清晰。不过扇区前端同样被增强，包括腔室内的噪声，因此该部分区域对比度降低。</small></b></p>
</td>
</tr>

</table>
</div>

## 时间增益补偿
---
现今所有的商业超声设备上都具有时间增益补偿（time gain compensation，TGC）功能。即根据传输时间对反射信号进行增益补偿，相当于根据距离进行增益补偿。不过这种方案并不理想，如果噪声不随着深度衰减或者随着深度衰减的程度不一，可能降低信噪比，导致某个深度产生灰度模糊。好处是能够获得更为平衡的图像，而且能够补偿大部分衰减。在谐波成像出现之前，TGC是可以调节的。由于谐波成像去除了很大部分腔室噪声，现在大多数设备采用自动TGC，但是保留了手动设置选项。时间增益补偿是一项预处理功能，必须在获得图像的同时进行处理。
<div><table style="text-align: center; width: 100%;" border="1" cellpadding="1" cellspacing="1">

<tr>
<td><p><small><b>下图为TGC控制按钮。每个滑动按钮控制某个深度上的增益水平  </b></small></p></td>
<td><p><small><b>在老旧型号上，为了获得平衡图像必须手动设置TGC!  </b></small></p></td>
</tr>

<tr>
<td><img src="https://github.com/WindTom/imagestom/blob/master/TGC1.jpg?raw=true"></td>
<td><img src="https://github.com/WindTom/imagestom/blob/master/TGC2.jpg?raw=true"></td>
</tr>

<tr>
<td><img src="https://github.com/WindTom/imagestom/blob/master/attenuation.jpg?raw=true"></td>
<td><img src="https://github.com/WindTom/imagestom/blob/master/attenuation%20tgc.jpg?raw=true"></td>
</tr>

<tr>
<td><p><small><b> 如今的设备都有自动TGC，所以为获得平衡图像滑动按钮都应该放在中间</b></small></p></td>
<td><p><small><b>按照老习惯设置按钮会导致双倍增益补偿，造成低端增益过大，而顶端又太小 </b></small></p></td>
</tr>

<tr>
<td><img src="https://github.com/WindTom/imagestom/blob/master/TGC1.jpg?raw=true"></td>
<td><img src="https://github.com/WindTom/imagestom/blob/master/TGC2.jpg?raw=true"></td>
</tr>


<tr>
<td><img src="https://github.com/WindTom/imagestom/blob/master/attenuation%20tgc.jpg?raw=true"></td>
<td><img src="https://github.com/WindTom/imagestom/blob/master/double%20tgc.jpg?raw=true"></td>
</tr>

</table></div><br>

## 压缩和拒绝

低振幅信号可以通过拒绝函数（reject function）过滤掉，比如过滤腔室噪声，代价是可能丢失有用的低振幅信号。

最后，灰度级被压缩（compressed）制造出更加陡峭的饱和曲线（saturation curve）。意思就是图像在低振幅达到全饱和（纯黑，**注意原文说纯白，但我理解的是纯黑**），同时低振幅的亮度被降低。

需要注意的是本节提到的都是后处理函数，不会改善信号质量或原始信噪比。

<div align="center"><table style="text-align: center; width: 100%;" border="1"  cellspacing="1">

<tr>
<td width="50%"><img src="https://github.com/WindTom/imagestom/blob/master/default.jpg?raw=true"></td>
<td width="50%"><img src="https://github.com/WindTom/imagestom/blob/master/Gain%20and%20reject.GIF?raw=true"></td>
</tr>

<tr>
<td><p><small><b>默认增益、拒绝和压缩设置下得到的图像  </b></small></p></td>
<td><p><small><b>增益、拒绝和压缩原理。所有曲线反映的都是亮度和被拒绝信号振幅之间的关系。黑色曲线为常规增益。提高增益补偿（红色曲线）会提高包括噪声以及灰度最弱的点在内的所有信号的强度，缺点是除了增强信号外，强度值较大的信号达到饱和值，可能导致图像的细节消失。蓝色曲线表示的是压缩，压缩能够产生更为陡峭的亮度曲线，让本来灰暗的信号更加灰暗，明亮的信号更加明亮，因此弱信号可能同背景噪声一起消失，而强信号达到饱和值，导致细节丢失。最后，浅灰色区域表示的是拒绝操作，该操作将低于某一阈值的所有信号都设为黑色。结合高增益补偿和拒绝操作将会产生同压缩操作类似的效果.</b></small></p></td>
</tr>

<tr>
<td><img src="https://github.com/WindTom/imagestom/blob/master/higain.jpg?raw=true"></td>
<td><img src="https://github.com/WindTom/imagestom/blob/master/reject.png?raw=true"></td>
</tr>

<tr>
<td><img src="https://github.com/WindTom/imagestom/blob/master/logain.jpg?raw=true"></td>
<td><img src="https://github.com/WindTom/imagestom/blob/master/compr.jpg?raw=true"></td>
</tr>

<tr>
<td><p><small><b>相同图像采用高增益补偿（上图）提高了心内膜的亮度，但亮度饱和导致细节丢失以及腔室内噪声增大。低增益补偿（下图）导致腔室内噪声下降，但细节丢失（如侧壁心内膜） </b></small></p></td>
<td><p><small><b>  相同图像采用高拒绝（上图）操作导致图像腔室噪声降低，但同样细节丢失（侧壁心内膜）。压缩操作（下图）提高了亮度导致心肌细节减少</b></small></p></td>
</tr>

</table>
<br>
</div>


