---
layout: post
title: 在 Arch Linux 中玩转 Hyprland
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
date: 2024-04-06 04:36 +0800
---
> 生命不息，折腾不止。
> 这是一篇流水账，非教程。

## 安装 Arch Linux

先聊聊为什么我最后选择了 Hyprland。B 战 up 主 TheCW 的 Discord server 貌似一致推崇 Hyprland，Nvchad 的作者 siduck 和他的 Discord server 一致推崇 i3wm。原因呢，因为在目前阶段 i3wm 仍然是比 Hyprland 更优秀的存在，因为相比 Hyprland 所用的 Wayland，i3wm 所用的 X 更稳定，虽然很老了，但是文档全面，你能遇到的几乎任何问题都能搜到对应的答案。而 Hyprland 相对来说比较新，支持会没有 i3wm 丰富。看了一圈油管视频，博主们都反应 Hyprland 的开发非常积极，反馈的问题几乎能当天解决。而且 Hyprland 使用的是更新的 Wayland 技术，用大佬的话来说，"连开发者都建议使用 Wayland"。

带着“越大的佬越有有话语权”的偏见，我先尝试了 i3wm。发现了如下问题：

- 刚入门 arch，虽然已经有了心理预期，但是还是被“所有都得自己 DIY”折腾到了。什么意思呢？
- 进入 i3wm，没有壁纸。搜了一圈可以使用 nitrogen。
- 没有 status bar，收到了不同的推荐，比如 polybar，i3wm 官推的 i3status 等等。
- 没有蓝牙，没有音频，没有网络，所有都得自己去找包，找教程。
- 没有设置界面，软件启动界面等等。收到了不同的推荐，比如 rofi，dmenu。
- 没有 power management，没有电池电量，没有登录界面，我是通过`startx`，也就是 xorg 的工具 xinit 启用的 i3wm。
- 不知道如何设置分辨率，Google 了一圈，可以使用`xrandr`设置分辨率，当然也是通过命令行。
- 没有文件管理系统，可以用 dolphin(from KDE)或者 nautilus(from Gnome)。

当然这些都不是根本原因，本着"买新不买旧"的精神，我最后决定"用新不用旧"。

尝试了用[archfi](https://github.com/MatMoul/archfi)安装 Arch Linux，也可以参考我写的[这篇文章](https://blog.techwen.cn/posts/install-arch-linux/)。装完 Arch 最基本的就行（Minimal Install）。最近又发现连网后可以直接`archinstall`，貌似是官方提供的安装脚本，后续我发现这个方法更加方便。不仅可以装好一些基础的必备工具，比如 NetworkManager，音频所需要的 Pipewire，还能同时装上一些自定义的桌面环境，比如 KDE，GNOME，i3wm，或者 Hyprland。

## Ricing

### 安装 Hyprland

在 GitHub 上发现了好评 3k+的[这个项目](https://github.com/prasanthrangan/hyprdots)，在 Hyprland 的基础上进行的 Rice。

```bash
cd # wherever of your choice
git clone --depth 1 https://github.com/prasanthrangan/hyprdots
cd /path/to/your/choice/Scripts
./install.sh
```

之所以选择这个脚本，主要是它包含了我日常所需的一切。比如 rofi 启动器，蓝牙，网络，音频模块。我懒得很，不想自己弄。
不好的一点是我的终端是用的 Alacritty，而这个脚本用的 Kitty。

### 配置 Hyprland

设置`hyprland.conf`，这里只覆盖我需要的一些配置。

**Capslock 与 Ctrl 互换**

```bash
input {
    ...
    kb_options = ctrl:swapcaps
    ...
}
```

**Key Repeat**

```bash
input {
    ...
    repeat_delay = 300
    repeat_rate = 50
    ...
}
```

`kb_options`可以参考`/usr/share/X11/xkb/rules/base.lst`。里面有很多有用的选项。

### Firefox CSS

我用的这个，[cascade](https://github.com/andreasgrafen/cascade)。将这个 repo 拉下来。

在 Firefox 进入`about:config`，搜索 `toolkit.legacyUserProfileCustomizations.stylesheets`，设为 true。进入`about:support`，里面找到对应的 profile folder，将 chrome 文件夹拷进去。然后进去设置就行了。

### 安装 Fcitx5 和 Rime 输入法

```bash
yay -S fcitx5-im fcitx5-rime
git clone --depth=1 https://github.com/Mark24Code/rime-auto-deploy.git --branch latest
cd rime-auto-deploy
./installer.rb # 按指示选
vim ~/.config/hyprland/hyprland.conf # 修改 hyprland 配置文件
exec-once = fcitx5 -d # 加在配置文件里
```

然后登出再登入

**安装 Fcitx5 输入法主题**

我用的[catppuccin](https://github.com/catppuccin/fcitx5)。

```bash
git clone https://github.com/catppuccin/fcitx5.git
mkdir -p ~/.local/share/fcitx5/themes/
cp -r ./fcitx5/src/* ~/.local/share/fcitx5/themes
```

然后在输入法的 GUI 设置选择主题就好了。
