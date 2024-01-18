---
layout: post
title: 记一次（被）折腾Arch Linux
description: 记录一次折腾Arch Linux的过程，包括安装、配置、美化等。
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
date: 2024-01-19 01:09 +0800
---
> 生命不息，折腾不止。

为了折腾Arch Linux，特地在闲鱼淘了一台联想小新13pro，2020款。托人从国内带了过来，不得不说找人带比转运美妙万倍。

[配置](https://detail.zol.com.cn/notebook/index1321767.shtml)如下：

- Rzyen 5 4600U
- 集显
- 16GB内存
- 512GB SSD

## 安装Arch Linux

首先参考了 [这篇博文](https://itsfoss.com/install-arch-linux/)

> 一定得开广告拦截器，不然这玩意儿没法看。
> 下一次，试试[archfi](https://github.com/MatMoul/archfi)，参考[这篇博文](https://niekvanleeuwen.nl/2020/08/making-arch-linux-i3-wm-usable/)

先去[官网](https://archlinux.org/download/?ref=itsfoss.com)下载最新的Arch Linux镜像制作启动U盘，我是用的[balenaEtcher](https://etcher.balena.io/)。

基本上照着博文来，这里只记录一些坑：

- 联想小新进入BIOS的方式：Fn + F12
- BIOS中关闭Secure Boot，不然镜像加载不出来，直接就进了Windows界面。
- `fdisk /dev/nvme0n1` 之后先`d`删除已经存在的分区，只要不`w`，如果中途退出了是不会保存的。
- 教程说要创建一个EFI分区因为小新pro有UEFI，但是安装完成后开启Secure Boot进不了系统，这个坑待解决。
- `ip link`确认有无检测到网卡，发现`wlan0`是DOWN的状态，这时候无法用`iwctl`连接wifi。`ip link set wlan0 up`启用网卡，如果报错`Operation not possible due to RF-kill`，就`rfkill unblock all`然后再启用网卡。最后再用`iwctl`连接wifi就行了，步骤参考博文。
- `timedatectl list-timezones`设置时区的时候报了错，去[官方wiki](https://wiki.archlinux.org/title/installation_guide)发现`arch-chroot /mnt`之后`timedatectl`用不了是正常的（其他命令也一样，原因有个Reddit说了但是忘记了来源），这里照官方文档解决：`ln -sf /usr/share/zoneinfo/Region/City /etc/localtime`
- `nano /etc/locale.gen`的时候可以多选几个，比如我把`zh_CN`相关的都选上了。

## pacman我都装了些啥？

这里从安装桌面环境开始

```bash
pacman -S xorg networkmanager
pacman -S gnome
```

> 等半天。。。

然后参照博文完成最开始的装机，接着开始装别的。

### 安装yay

`yay`是Arch Linux管理AUR，也就是社区包的管理工具。装软件基本上都用得到。这里参考了[这篇博文](https://itsfoss.com/install-yay-arch-linux/)。

```bash
sudo pacman -Syu
sudo pacman -S --needed base-devel git
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
yay --version # 搞定
```

> With the `--needed flag`, it will NOT reinstall the already installed packages.

`yay`的逻辑基本上就是`build from source`，但是给他命令化了。意思就是，没有`yay`，如果我们要装一个软件：

```bash
git clone <软件的git地址，通常是AUR提供>
cd <软件名>
makepkg -si # build from source
```

有了`yay`，就可以简化上述过程：

```bash
yay -S <软件名>
```

#### yay的基本使用

> 这里直接抄了

| Command                 | Description                                        |
| ----------------------- | -------------------------------------------------- |
| `yay search_term`       | Search for packages using `yay`.                   |
| `yay -S package_name`   | Install the specified package.                     |
| `yay -R package_name`   | Remove the specified package.                      |
| `yay -Rns package_name` | Remove the specified package and its dependencies. |
| `yay -Sua`              | Upgrade AUR (Arch User Repository) packages only.  |
| `yay -Sua`              | Upgrade Yay to a new version.                      |
| `sudo pacman -Rs yay`   | Remove Yay from your Arch system.                  |

### 安装中文字体

> 没装中文字体前，会显示乱码。

```bash
sudo pacman -S wqy-microhei wqy-zenhei
# 或者
sudo pacman -S noto-fonts noto-fonts-cjk
```

### 安装RIME输入法

> 因为后来试了一下Hyprland，但是Hyprland对ibus-rime，也就是中州韵官方的版本支持等于不存在，所以不得不换到Fcitx5-rime。参考后文。

我用的[雾凇拼音](https://github.com/iDvel/rime-ice)。
懒得自己配置，所以用了作者提供的一个脚本，参考[rime-auto-deploy](https://github.com/Mark24Code/rime-auto-deploy)。

```bash
sudo pacman -S ruby # 要先装ruby
sudo pacman -S ibus-rime # 要自己先装中州韵的输入法框架，可以用官方的也可以用第三方的，我用的官方的。
git clone --depth=1 https://github.com/Mark24Code/rime-auto-deploy.git --branch latest
cd rime-auto-deploy
./installer.rb # 然后按指示选
```

安装完成后要登出，然后重新登录Gnome。搞定。

### 安装杂七杂八

```bash
sudo pacman -S firefox # firefox
# 几款我喜欢的nerd font
sudo pacman -S ttf-firecode-nerd-font
sudo pacman -S ttf-hack-nerd
sudo pacman -S ttf-meslo-nerd
yay -S neovim-nightly-bin # 我用的v.10.0 的nightly版本的neovim
sudo pacman -S neovim # 或者用官方的stable版本
sudo pacman -S alacritty # 御用Terminal
sudo pacman -S nodejs npm # nodejs
sudo pacman -S eza # eza
# 配置eza的alias至.zshrc
alias l="eza -la --icons=always"
alias ls="eza -a --icons=always"
sudo pacman -S tmux # tmux
# 配置tmux的alias至.zshrc
alias tn="tmux new -s"
alias tl="tmux ls"
alias ta="tmux attach -t"
alias td="tmux detach"
sudo pacman -S neofetch # neofetch
```

### 开始折腾zsh

```bash
sudo pacman -S zsh # 安装zsh
which zsh # 查看zsh装好了没
sudo chsh -s $(which zsh) # 设置zsh为默认SHELL
```

> 完事后要登出Gnome再登入，再执行`echo $SHELL`看成功了咩。

```bash
# 安装oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# 安装 zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
# 安装 zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
# 安装 powerlevel10k
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

完事之后修改`.zshrc`

```bash
ZSH_THEME="powerlevel10k/powerlevel10k"

plugins=(
	git
	zsh-syntax-highlighting
	zsh-autosuggestions
	z # oh-my-zsh自带
)
```

`source .zshrc`，搞定。最后执行`p10k configure`，按着提示来。

### 把Jekyll博客搭起来

[官方文档](https://jekyllrb.com/docs/installation/other-linux/)

```bash
sudo pacman -S ruby base-devel --needed
# 编辑.zshrc
export GEM_HOME="$(ruby -e 'puts Gem.user_dir')"
export PATH="$PATH:$GEM_HOME/bin"
# 然后
source ~/.zshrc
gem install jekyll bundler
```

#### 天坑

`bundle exec jekyll serve`报

```console
bundler: failed to load command: jekyll (/home/alamgir/.local/share/gem/ruby/3.0.0/bin/jekyll)
/home/alamgir/.local/share/gem/ruby/3.0.0/gems/jekyll-4.3.3/lib/jekyll.rb:29:in `require': cannot load such file -- json (LoadError)
...
```

搞了几个小时没搜到结果，`gem list json`有结果，`bundle exec gem list json`没结果。
`jekyll serve`可以，`bundle exec jekyll serve`不行。搜了大半个Google，找到一个workaround。
在Gemfile里面加入。

```ruby
gem 'json'
```

再`bundle install`，搞定。

## 后话

### 关于wifi

因为我用的是[NetworkManager](https://wiki.archlinux.org/title/NetworkManager)，这个工具自带了一个命令行工具`nmtui`，用这个工具先连上wifi。然后NetworkManager的wiki里面推荐了[network-manager-applet](https://archlinux.org/packages/?name=network-manager-applet)，这是一个system tray applet，也就是在状态栏提供一个托盘工具。

`yay -S network-manager-applet`，然后执行`nm-applet --indicator`，桌面右上角会有一个wifi托盘可以用来设置网络连接。

还有一种方式是用`nmcli`[命令](https://developer-old.gnome.org/NetworkManager/stable/nmcli.html)连接wifi，也是NetworkManager自带的。先执行`nmcli device wifi`搜索可用的wifi，然后`nmcli device wifi connect <wifi名> password <密码>`连接。

### 安装Fcitx5和Rime输入法

```bash
yay -S fcitx5-im fcitx5-rime
git clone --depth=1 https://github.com/Mark24Code/rime-auto-deploy.git --branch latest
cd rime-auto-deploy
./installer.rb # 按指示选
vim ~/.config/hyprland/hyprland.conf # 修改hyprland配置文件
exec-once = fcitx5 -d # 加在配置文件里
vim ~/.zshrc # 修改.zshrc配置加入如下
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```

第一次运行要取消勾选Input Method页的Only show current language，然后找到RIME ，设置为默认键盘然后删掉默认键盘。

设置输入框的DPI:

fcitx5-configtool->Addons -> Classic User Interface -> ✅ Use Per Screen DPI

fcitx5-configtool->Addons -> Classic User Interface -> Force Font DPI on Wayland 144

### 后续计划：

- 重装一次，用archfi。
- 试试i3wm。
