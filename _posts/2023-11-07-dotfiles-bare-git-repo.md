---
layout: post
title: 利用github管理dotfiles
date: 2023-11-07 07:04 +0800
description: 如何利用bare git repo （裸仓库）管理配置文件
image: https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202311062309166.jpg
category:
  - 不学无术
tags:
  - git
toc: true
comments: true
math: false
mermaid: false
pin: false
sitemap: true
published: true
lang: zh-CN
---

## 如何使用git bare仓库管理dotfiles？

### 初始化一个bare git repo

```bash
git init --bare $HOME/github/dotfiles

```

> 路径可以自定义

### 添加alias到.zshrc (或.bashrc，取决与你的shell）

```bash
alias config='$(which git) --git-dir=$HOME/github/dotfiles/ --work-tree=$HOME'
```

`source .zshrc`

### 设置flag隐藏不需要追踪的文件

设置一个flag --local 以隐藏尚未明确跟踪的文件。
这样，当键入 `config status` 和其他命令时，我们不感兴趣跟踪的文件将不会显示为
`untracked`

```bash
config config --local status.showUntrackedFiles no
```

### 开始利用git追踪文件

```bash
config add .zshrc
config status
config commit -m "add .zshrc"
```

创建一个空的github repo，并配置origin

```bash
config remote add git@github.com:minicoderwen/dotfiles.git
```

`push`到远程分支

```bash
config push -u origin main
```

## 恢复dotfiles到新系统

```bash
alias config='/usr/bin/git --git-dir=$HOME/github/dotfiles --work-tree=$HOME' # 添加到.zshrc
source .zshrc # 使alias生效
echo "github/dotfiles" >> .gitignore # 添加dotfile目录到gitignore
git clone --bare <git-repo-url> $HOME/github/dotfiles # 克隆裸仓库到本地dotfile目录
config config --local status.showUntrackedFiles no
config checkout
```

如果出现类似如下错误，表明本地目录已经有文件存在了

```bash
error: The following untracked working tree files would be overwritten by checkout:
    .bashrc
    .gitignore
Please move or remove them before you can switch branches.
Aborting
```

可以使用如下脚本备份本地已经存在的配置文件到 `.config-backup`

```bash
git clone --bare https://bitbucket.org/durdn/cfg.git $HOME/.cfg
function config {
   /usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME $@
}
mkdir -p .config-backup
config checkout
if [ $? = 0 ]; then
  echo "Checked out config.";
  else
    echo "Backing up pre-existing dot files.";
    config checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | xargs -I{} mv {} .config-backup/{}
fi;
config checkout
config config status.showUntrackedFiles no
```

[参考链接](https://www.atlassian.com/git/tutorials/dotfiles)
