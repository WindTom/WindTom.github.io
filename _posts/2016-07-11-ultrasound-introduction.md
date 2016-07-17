---
layout: post
title: "超声、超声心动图和多普勒基础入门（一）"
description: ""
category: ultrasound
tags: [ultrasound]
---
{% include JB/setup %}


----------

英文链接: 
（http://folk.ntnu.no/stoylen/strainrate/Basic_ultrasound#ultrasound） 
本文的目的是尽量以图片介绍超声心动图和多普勒基础知识。

题外话：看老外的科普性的文章，最大的感受就是直观，很容易让人明白，而国人总喜欢取术语，而且取名又让人很难理解其中的含义。百度上搜一个问题，不同的博客给的都是同一个答案。如果我们都能少一些拷贝粘贴，多写一些理解性的话，那别人学起来就容易多了。

###  超声   ###
简单来说，超声就是声波，和人的声音一样，只是你听不见而已。声音是纵波，沿声音传播方向前后振荡。 
人能听见的声音大概在15000到20000赫兹，而临床上超声的频率在1000到12000赫兹。我们能墙角拐弯处能听见声音，是因为声音在这些弯角处发生衍射；高频（短波长）声音则会像电磁波一样沿直线传播，遇到物体时反射回来的波束能量也更为集中，就像光束的反射一样。因为波长短，高频声波更易在较小的物体上发生发射，且在气态媒介中不易传播。 
高中物理课本告诉我们波长和频率f成反比，它们与速度v的关系是:


<img src="https://github.com/WindTom/imagestom/blob/master/gongshi.gif?raw=true">

声波在不同媒介中的速度不同。

脉冲电压加在晶片上，晶体产生振动，从而产生超声波，我们叫这种晶体为压电晶体。同一个晶体可以作为反射超声波的接收者。

什么是超声数据？ 
超声数据按照复杂度可作如下分类：
<table style="text-align: left; width: 100%;" border="0" cellpadding="2" cellspacing="2">
      <tbody>
        <tr>
          <td style="vertical-align: top; text-align: center;"><img style="width: 261px; height: 249px;" alt="" src="https://github.com/WindTom/imagestom/blob/master/RF.GIF?raw=true" height="249" width="261"><br>
          </td>
          <td style="vertical-align: top; text-align: center;"><img style="width: 261px; height: 249px;" alt="" src="https://github.com/WindTom/imagestom/blob/master/RF-ampl.GIF?raw=true"><img style="width: 261px; height:
              249px;" alt="" src="https://github.com/WindTom/imagestom/blob/master/ampl.GIF?raw=true"><br>
          </td>
          <td style="vertical-align: top; text-align: center;"><img style="width: 261px; height: 249px;" alt="" src="https://github.com/WindTom/imagestom/blob/master/freq.GIF?raw=true" height="249" width="261"><br>
          </td>
        </tr>
        <tr>
          <td style="vertical-align: top;"><small><span style="font-weight: bold;">Basically, a reflected
                ultrasound pulse is a waveform. However, storing the
                full waveform, called RF data, is demanding in terms of
                storage, as each point on the curve would have to be
                represented in some way or other. However, if the full
                RF data are stored, the amplitude and frequency data
                could both be calculated in post processing. </span></small><br>
          </td>
          <td style="vertical-align: top;"><small><span style="font-weight: bold;">The pulse has a certain
                amplitude. Just storing the amplitude is much les
                demanding (corresponding more or less to one number per
                pulse). This is the only data that are used in grey
                scale imaging, where the amplitude is displayed as
                brightness of the point correspåonding to the scatterer
                as in <a href="#2D">B-mode</a> and <a href="#M-mode">M-mode</a>.</span></small><br>
          </td>
          <td style="vertical-align: top;"><small><span style="font-weight: bold;">However, the reflected
                ultrasound pulse has a frequency (or a spectrum of
                frequencies), and this can be represented as a numerical
                value per image pixel as well, as described in <a href="#Doppler">Doppler </a>imaging. Still, the
                amount of data is far less than the RF data. </span></small><br>
          </td>
        </tr>
      </tbody>
    </table>






<div id="disqus_thread"></div>
<script>
    /**
     *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
     *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables
     */
    /*
    var disqus_config = function () {
        this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
        this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    */
    (function() {  // DON'T EDIT BELOW THIS LINE
        var d = document, s = d.createElement('script');
        
        s.src = '//windtom.disqus.com/embed.js';
        
        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>