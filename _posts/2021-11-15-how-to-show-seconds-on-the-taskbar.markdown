---
layout: post
title: How to Show Seconds on the Taskbar
author: Kelly
tags: [Windows, 软件使用]
date: 2021-11-15 01:33:33 +0800
published: true
excerpt_separator: <!--more-->
---

Windows 任务栏的时间区域，缺省只显示到分钟数，有时候需要看到秒数，就显得不方便。  
<!--more-->
1. 按 `Win + R` 打开 `运行窗口`，输入 `regedit` 后打开 `注册表编辑器`，找到 `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced` 位置
![](/img/2021-11-15_8-58-51.png)  
![](/img/2021-11-15_8-51-06.png)  
2. 鼠标右键点击 `Advanced`，创建 `DWORD (32位)值`，数值名称为：`ShowSecondsInSystemClock`，数值数据为：`1`。
![](/img/2021-11-15_9-04-10.png)
![](/img/2021-11-15_8-52-37.png)  
3. 关闭 `注册表编辑器`，打开 `任务管理器`，重新启动 `Windows 资源管理器`。
![](/img/2021-11-15_9-09-04.png)
4. 任务栏上的时间区域即显示出秒数。
![](/img/2021-11-15_8-56-08.png)  

# 环境
- Windows 10 专业版
