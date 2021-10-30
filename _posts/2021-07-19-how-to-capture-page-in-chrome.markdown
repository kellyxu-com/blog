---
layout: post
title: 如何在 Google Chrome / Microsoft Edge 中截图
author: ray
tags: [Linux]
date: 2021-07-19 02:33:33 +0800
published: true
excerpt_separator: <!--more-->
---

<!--more-->
[toc]

按 `F12`（macOS 是 `option + command + i`）调出开发者工具，接着按 `Ctrl + Shift + P`（macOS 是 `command + Shift + P`）。紧接着输入指令 `capture`（Edge 是输入 `截图`），它会提示有四个选项，如下图所示：
![](rayspace/assets/img/2021-07-19_9-17-22.png)  

# 1. Capture screenshot
截取屏幕可见区域的网页  
![](rayspace/assets/img/kellyxu.com_screen.png)  

# 2. Capture node screenshot
- 在开发者工具中选中 DOM 一个节点，然后截图：
![](rayspace/assets/img/kellyxu.com_node.png)  

- 如果选择根部一点的节点，然后截图，相当于截取整个网页：
![](rayspace/assets/img/kellyxu.com_node2.png)  

# 3. Capture full size screenshot
截取整个网页，含没有显示在屏幕窗口的部分
![](rayspace/assets/img/kellyxu.com_full_screen.png)  

# 环境
- Google Chrome 版本 91.0.4472.164（正式版本）（64 位）
- Microsoft Edge 92.0.902.67 (官方内部版本) (64 位)
- Windows 10 专业版
