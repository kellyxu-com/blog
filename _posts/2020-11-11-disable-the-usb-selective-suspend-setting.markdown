---
layout: post
title: Disable USB Selective Suspend in Window 10 via Control Panel
author: kelly
tags: [Windows, 软件使用]
date: 2020-11-11 01:33:33 +0800
published: true
excerpt_separator: <!--more-->
---

Windows 10 笔记本中使用 U 盘或者连接一些 USB 外设的情况下，时不时就有设备失灵的现象。经过研究发现，微软为了节省电力，在 Windows 10 系统中内置了一项「USB 选择性暂停设置」，而这个功能默认还处于启用状态。  
<!--more-->
禁用即可。  
![](/img/2020-11-11_14-43-34.png)  
![](/img/2020-11-11_14-44-09.png)  
发现还是U盘还是会掉线，打开`设备管理器`—`通用串行总线控制器`，选中某控制器，点击鼠标右键，打开`属性`—`电源管理`，去掉`允许计算机关闭此设备以节约电源`前的勾，试试看。  
![](/img/2020-11-28_21-47-34.jpg)  

# 环境
- Windows 10 专业版
