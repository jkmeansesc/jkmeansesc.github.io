---
layout: post
title: 在Arch Linux中玩转Hyprland
description: Ricing Arch Linux with Hyprland.
image: https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401161203859.jpg
category:
  - 不学无术
tags:
  - arch
toc: true
comments: true
math: false
mermaid: false
pin: false
sitemap: true
published: true
lang: zh-CN
---

> 生命不息，折腾不止。
> 这是一篇流水账，非教程。

## 安装Arch Linux

先聊聊为什么我最后选择了Hyprland。B战up主TheCW的Discord server貌似一致推崇Hyprland，Nvchad的作者siduck和他的Discord server一致推崇i3wm。原因呢，因为在目前阶段i3wm仍然是比Hyprland更优秀的存在，因为相比Hyprland所用的Wayland，i3wm所用的X更稳定，虽然很老了，但是文档全面，你能遇到的几乎任何问题都能搜到对应的答案。而Hyprland相对来说比较新，支持会没有i3wm丰富。看了一圈油管视频，博主们都反应Hyprland的开发非常积极，反馈的问题几乎能当天解决。而且Hyprland使用的是更新的Wayland技术，用大佬的话来说，"连开发者都建议使用Wayland"。

带着“越大的佬越有有话语权”的偏见，我先尝试了i3wm。发现了如下问题：

- 刚入门arch，虽然已经有了心理预期，但是还是被“所有都得自己DIY”折腾到了。什么意思呢？
- 进入i3wm，没有壁纸。搜了一圈可以使用nitrogen。
- 没有status bar，收到了不同的推荐，比如polybar，i3wm官推的i3status等等。
- 没有蓝牙，没有音频，没有网络，所有都得自己去找包，找教程。
- 没有设置界面，软件启动界面等等。收到了不同的推荐，比如rofi，dmenu。
- 没有power management，没有电池电量，没有登录界面，我是通过`startx`，也就是xorg的工具xinit启用的i3wm。
- 不知道如何设置分辨率，Google了一圈，可以使用`xrandr`设置分辨率，当然也是通过命令行。
- 没有文件管理系统，可以用dolphin(from KDE)或者nautilus(from Gnome)。

当然这些都不是根本原因，本着"买新不买旧"的精神，我最后决定"用新不用旧"。

尝试了用[archfi](https://github.com/MatMoul/archfi)安装Arch Linux，也可以参考我写的[这篇文章](https://blog.techwen.cn/posts/install-arch-linux/)。装完Arch最基本的就行（Minimal Install）。最近又发现连网后可以直接`archinstall`，貌似是官方提供的安装脚本，后续我发现这个方法更加方便。不仅可以装好一些基础的必备工具，比如NetworkManager，音频所需要的Pipewire，还能同时装上一些自定义的桌面环境，比如KDE，GNOME，i3wm，或者Hyprland。

## Ricing

### 安装Hyprland

在Github上发现了好评3k+的[这个项目](https://github.com/prasanthrangan/hyprdots)，在Hyprland的基础上进行的Rice。

```bash
cd # wherever of your choice
git clone --depth 1 https://github.com/prasanthrangan/hyprdots
cd /path/to/your/choice/Scripts
./install.sh
```

之所以选择这个脚本，主要是它包含了我日常所需的一切。比如rofi启动器，蓝牙，网络，音频模块。我懒得很，不想自己弄。
不好的一点是我的终端是用的Alacritty，而这个脚本用的Kitty。

### Capslock与Ctrl互换

这是个人习惯。
