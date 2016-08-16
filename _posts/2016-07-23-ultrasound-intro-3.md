---
layout: post
title: "超声、超声心动图和多普勒基础入门（三）"
description: ""
category: ultrasound
tags: [ultrasound]
---
* content
{:toc}
英文原文链接：   
（http://folk.ntnu.no/stoylen/strainrate/Basic_ultrasound#ultrasound）

## M-mode

M型超声是能够记录并显示心脏运动的第一种超声设备。下图为典型的M-型超声图像。




<div>
<table style="text-align: center; width: 100%;" border="1" cellpadding="1" cellspacing="1">

<tr>
<td colspan="3"><img src="https://github.com/WindTom/imagestom/blob/master/vvpslax.JPG?raw=true"></td>
</tr>

<tr>
<td><img src="https://github.com/WindTom/imagestom/blob/master/vvmm.JPG?raw=true"></td>
<td><img src="https://github.com/WindTom/imagestom/blob/master/mvmm.JPG?raw=true"></td>
<td><img src="https://github.com/WindTom/imagestom/blob/master/aomm.JPG?raw=true"></td>
</tr>

<tr>
<td>a</td>
<td>b</td>
<td>c</td>
</tr>

<tr>
<td colspan="3"><p><small><b>图：典型的M-型超声成像。a是左心室成像。b是二尖瓣成像。c是主动脉血管成像（二维长轴）。这里振幅表现为白色的亮度。</b></small></p></td>
</tr>
</table>
<br>
</div>

## 深度分辨率、带宽

超声波深度分辨率指的是沿波束方向的分辨率，这取决于发射出来的超声脉冲长度。在血液和组织交界处能够看到一条明亮的分界线，但这反映出来的是脉冲长度，并不是组织结构（尤其不是指内膜，因为内膜太薄，现有超声频率根部看不到）。也是这个原因，美国超声心动协会（ASE）规定用超声的前锋到前锋距离作为深度。（这句话不好懂，也许看完下面的内容会明白）

### 关于超声脉冲
这里稍微插一段。你可能不知道上面一直在讲超声波，这里怎么又出现超声脉冲。好了，[维基百科](http://www.ultrasonix.com/wikisonix/index.php/Transmitting_Ultrasound_Pulses)是这么解释的：

超声波脉冲是能够在生物体组织中传播的机械波。要制造出这种脉冲，需使用超声换能器（ultrasound transducer），它能将机械能转换成电能，同样也能将电能转换为机械能。

<div><hr>
</div>

**时间上的超声波脉冲形状**

获得组织图像的超声波有两种类型：

脉冲波![](http://www.ultrasonix.com/wikisonix/images/thumb/2/2d/Pulse.jpg/300px-Pulse.jpg)  
连续波![](http://www.ultrasonix.com/wikisonix/images/thumb/e/e8/CW.jpg/300px-CW.jpg)

脉冲波的获取方式为：给超声换能器施加短时间电信号（电脉冲）。没有换能器能够转换出与电脉冲形状一模一样的超声脉冲。决定发射出来的脉冲形状的因素不仅是施加的电脉冲形状，还有换能器对频率的反应。将电脉冲施加到换能器上就好比拿一根木棒敲钟，即使木棒不再敲打，钟还是会继续响。下图所示为换能器输入电脉冲，输出机械脉冲：

<div align="center">
<br>
<img src="http://www.ultrasonix.com/wikisonix/images/thumb/1/18/TransFiltering.jpg/450px-TransFiltering.jpg">
<br><br>
</div>

**[脉冲](http://www.cis.rit.edu/research/ultrasound/ultrasoundintro/ultraintro.html)**

频率、周期、波长和传播速度足以描述连续超声波。周期性循环，不断往复……。

超声波扫描术使用的是脉冲超声，它也是周期性的超声波，相邻周期之间具有间隔，而这段时间间隔之内没有信号。如下图：

<div align="center">
<img src="http://www.cis.rit.edu/research/ultrasound/ultrasoundintro/pulsed.png" width="70%">
<br>
<img src="http://www.cis.rit.edu/research/ultrasound/ultrasoundintro/pulsentissue.png" width="70%">  
<br>
<hr>
</div>

让我们回到深度分辨率、带宽的讨论。

理想情况下，图像（B型和M型超声）中的脉冲长度应该尽可能短，但这取决于探头的物理特性。Most probes will ring in the resonance frequency for a few oscillations, and thus produce a pulse with a length of several oscillations. By Fourier analysis, the frequency content of the pulse will be less dispersed, the longer the pulse is. Thus, the pulse length is inversely proportional with  the spread of the frequency, i.e. the bandwidth of the pulse, as shown below. This will have consequences for Doppler imaging, where frequencies, and not amplitudes are analysed. （这句话很难理解，feel it!）

<div align="center"><table style="text-align: center; width: 100%;" border="1" cellpadding="1" cellspacing="1">

<tr>
<td><img src="https://github.com/WindTom/imagestom/blob/master/Bandwidth.GIF?raw=true"></td>
</tr>

<tr>
<td><p><small><b> 图：具有相同频率、不同持续时间（脉冲长度或者说震荡次数）的两个不同脉冲。较短的脉冲具有更宽的频率色散（dispersion），也就是更大的带宽。</b></small></p></td>
</tr>

</table><br><br></div>


震荡次数相同的情况下表，更高的频率会导致脉冲更短，也就是说在不增加带宽的情况下缩短脉冲长度。对于成像来说，理想的脉冲应该是最高的频率，最短的脉冲长度。不过，由于噪声不均匀地分布在不同频域，谐波成像（which analyses at half the frequency）会减少噪声。给定频率，谐波成像将拉长脉冲长度，这样的结果是成像物体更厚。

<div align="center"><table style="text-align: center; width: 100%;" border="1" cellpadding="1" cellspacing="1">

<tr>
<td><img src="https://github.com/WindTom/imagestom/blob/master/pulse%20length%20vs%20frequency.GIF?raw=true"></td>
</tr>

<tr>
<td><p><small><b>频率降半导致单位时间内的震荡次数更少。像二次谐波成像中将频率减半会导致更长的脉冲长度，但带宽几乎不受影响。</b></small></p></td>
</tr>

<br></table>
</div>

<div align="center"><table style="text-align: center; width: 100%;" border="1" cellpadding="1" cellspacing="1">

<tr>
<td width="50%">
<img src="https://github.com/WindTom/imagestom/blob/master/2harmsurface.JPG?raw=true">
</td>
<td><img src="https://github.com/WindTom/imagestom/blob/master/leadingtoleading.JPG?raw=true"></td>
</tr>

<tr>
<td><p><small><b>左为二次谐波成像(1.7/3.5 MHz),右为原图像。由于谐波成像将频率减半，导致血液和隔膜交界处的回声(echo)更厚，但腔室内噪声明显减少。 </b></small></p></td>
<td><p><small><b>表面回声（surface echos）的厚度取决于脉冲长度和频率。这张隔膜的成像展示了ASE 的leading-edge to leading-edge 测量方法，方法中减去了魔宠长度。而Penn方法将两侧交界面都包含了进去，导致对厚度估计过高。（博主注：原文没有说清楚，导致此处理解不是很明白） </b></small></p></td>
</tr>
</table>
<br>
</div>

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>