> 这是我最近设计的一个HDMI转MIPI模块，可以用于驱动各种手机屏幕当显示器用。

## 有什么用？

大家知道现在的手机屏幕素质非常高，且价格低廉（毕竟有智能手机的普及量撑腰，作为维修配件买的话非常便宜），相比于绝大多数桌面显示器拥有十分无敌的分辨率、像素密度、可视角、色彩还原甚至刷新率。

大家又知道，我对于小巧精致的电子产品有执着的追求，可市面上几乎找不到用手机屏幕做的迷你显示器，所以本项目就是为了解决这个需求。至于迷你HDMI显示器有什么用，电视盒子、单反相机、树莓派之类的开发板都带HDMI接口，**即插即用随身携带的高分屏它不香吗？**

## 硬件原理

目前绝大多数的手机屏幕和小型高分辨率高刷新率屏幕基本都是MIPI接口，相比于RGB、LVDS、SPI等接口MIPI是一个非常强大的高速接口，它分为CSI和DSI两个规格（没错就是树莓派上预留的那个DSI），可以根据带宽需求自由配置lane数，且每个lane传输速率超过1Gbps。

而HDMI是最为常用的视频接口，几乎所有视频输出设备都会带一个HDMI接口。

**因此我们需要的就是一个HDMI转MIPI的硬件模块。**要实现这个目的可以有几种方案，走FPGA或者用ASIC芯片。

用FPGA的方案这里有个老哥开源了：https://hackaday.io/project/364-mipi-dsi-display-shieldhdmi-adapter

他用Spartan-6 FPGA成功驱动了iPhone4的屏幕并接受HDMI的信号输入，感兴趣的可以参考。

![](/4.Docs/images/1.jpg)

因为我对FPGA不是很熟，所以我采用ASIC专用IC的方案来设计。

#### 东芝方案

东芝有一款TC358870XBG芯片，支持2x4lane的屏幕驱动，输入源是HDMI，这是目前在AR眼镜中比较流行的一个方案，该芯片非常强大，但是缺点是资料极其稀缺。**我花了很长时间搞到了原厂的datasheet和相关文档，仓库里面都共享出来了。**

根据原厂的评估板我也设计了一个测试模块，电路已经开源在仓库。


![](/4.Docs/images/2.jpg)

这个方案的软件我还没有写，感兴趣的同学可以参考文档做后续开发，**也欢迎有进展的同学提交代码到仓库~**

> 目前东芝方案的固件代码已经由[ylj2000](https://github.com/ylj2000)基本实现了，器源码已经整合到本仓库，感谢[ylj2000](https://github.com/ylj2000)同学的开源代码~
>
> 大家可以去他的仓库具体了解：
>
> [ylj2000/HDMI_To_MIPI: A Hdmi to Mipi conversion module based on Toshiba TC358870 (github.com)](https://github.com/ylj2000/HDMI_To_MIPI)

#### 龙讯方案

国产还有一个龙讯方案LT6911，与上面的方案相比龙讯性能上稍弱一些，但是该芯片内置了一个51核的MCU，所以可以直接在片上编程（东芝的需要额外加一个单片机用I2C配置芯片）。

该方案的优点就是成本相对较低，芯片外围电路也更简洁，缺点是，**资料比东芝的还少...**

厂家不开放软硬资料，连datasheet都没有，所以几乎无法个人开发。**但是**，万能的野生钢铁侠通过一些特殊手段，还是跟代理商拿到了一些资料，包括部分源码（核心lib封装好了我拿不到，只有上层API）。但是因为签了NDA保密协议，源码部分我不好分享出来，除了源码其他部分我都开源了，而大家DIY的话也不需要源码，我可以提供预编译的固件供大家下载，所以这个方案适合给直接复制项目的同学参考。



![](/4.Docs/images/3.jpg)

![](/4.Docs/images/4.jpg)

最终驱动的效果如下，以5.5寸的屏幕为例：

![](/4.Docs/images/5.jpg)

## 总结

我后面还会继续用这个模块尝试驱动更多屏幕，**同时可能会量产一些比较方便的迷你显示器产品，有兴趣的同学可以关注一下~**

有开发能力的同学可以在我给出的东芝方案的基础上继续开发，这个方案的自由度会高很多，我后面有空也会继续完成这个方案的:D

#### 找资料和开发不易，记得给仓库点星星哈~~
