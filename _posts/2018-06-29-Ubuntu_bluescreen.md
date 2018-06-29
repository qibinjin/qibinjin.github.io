---
layout: post
cover: assets/images/changbaishan.jpg
title: (转)Ubuntu启动蓝屏的问题结局
date: 2018-06-28 21:21:00
tags: linux
author: Kim
---
<h4>转载地址：http://tieba.baidu.com/p/4898482611</h4>

<p>解决方法：</p>
<br>
<p>问题分析:启动 Ubuntu 可以进入登录界面，说明系统是可以运行起来的。没有发生大块的核心数据损坏，linux 系统一般都可以修复，一定要淡定。于是开始放狗（google）搜索。“VMware Ubuntu 蓝屏”“VMware Ubuntu 开机蓝屏”“VMware Ubuntu 登录蓝屏”依次搜索查看，找到的相关内容寥寥无几。多数是 Windows 蓝屏问题。后来看到一篇是讲登录后黑屏的问题的，尝试了其方法，有效！问题是之前的暴力关机损坏了 Ubuntu 的图形系统配置，导致图形界面无法正常起来。所以就看到能够登录，却只有一片蓝色。 问题解决这次要求助古老的字符界面了。为了“大展拳脚”，先进入字符界面：Ctrl + Alt + F4然后安装相应服务，然后重置它！sudo apt-get install xserver-xorg-lts-utopic sudo dpkg-reconfigure xserver-xorg-lts-utopic reboot如果前面第一个操作有问题，需要重置 dpkg 后再试，总之按提示操作就好了。sudo dpkg --configure -a有看到其他人不是安装 xserver-xorg-lts-utopic，而是 xserver-xorg-lts-quantal 。估计是版本问题。reboot 系统后，亲切的图形界面终于回来了</p>
<a href="assets/images/changbaishan(source).jpg">封面图片的高清大图</a>

