---
layout: post
title: How to Install Windows Sandbox
author: kelly
tags: [Windows, 软件使用]
date: 2021-07-21 01:33:33 +0800
published: true
excerpt_separator: <!--more-->
---

从网上下载的软件，有些是不明来源并且是临时使用的，考虑到该软件极有可能含有恶意代码，为安全起见，我们应该在 Windows 沙盒中运行它。

<!--more-->
> Windows 沙盒提供了轻型桌面环境，可安全地隔离运行应用程序。 Windows 沙盒环境中安装的软件保持"沙盒"状态，并独立于主机运行。
>
> 沙盒是临时的。 关闭后，将删除所有软件和文件以及状态。 每次打开应用程序时，都会获得沙盒的全新的实例。
>
> 安装在主机上的软件和应用程序不会直接在沙盒中提供。 如果需要特定应用程序在 Windows 沙盒环境中可用，则必须在环境中显式安装它们。
>
> Windows 沙盒具有以下属性：
>
> * Windows 的一部分：此功能所需的一切内容都包含在 Windows 10 专业版和企业版中。 无需下载 VHD。
> * Windows 沙盒：每次运行 Windows 沙盒时，它就像全新安装的 Windows 一样干净。
> * 可释放：设备上不会保留任何内容。 当用户关闭应用程序时，将放弃所有内容。
> * 安全：使用基于硬件的虚拟化进行内核隔离。 它依赖 Microsoft 虚拟机监控程序运行单独的内核，该内核将 Windows 沙盒与主机隔离。
> * 高效： 使用集成的内核计划程序、智能内存管理和虚拟 GPU。

1. 先打开`启用或关闭 Windows 功能`
![](/img/2021-07-21_15-56-14.png)

2. 勾选上`Windows 沙盒`，然后点击`确定`按钮，系统会提示重启。
![](/img/2021-07-21_15-56-55.png)  

3. 运行 Windows Sandbox
在任务条的搜索框中搜索`Windows Sandbox`，运行 Windows 沙盒。
![](/img/2021-07-21_16-22-11.png)

4. 可以通过`拷贝 / 粘贴`，将 Windows 机器上的文件，拷贝到沙盒运行。

# 参考
- [Windows 沙盒](https://docs.microsoft.com/zh-cn/windows/security/threat-protection/windows-sandbox/windows-sandbox-overview)

# 环境
- Windows 10 专业版 21H1，内部版本 19043.1110
