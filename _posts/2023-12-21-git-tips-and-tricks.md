---
layout: post
title: git 用过的招和踩过的坑
description:
image: https://raw.githubusercontent.com/minicoderwen/picwen/main/img/2023-10-05-1696543005.jpg
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
date: 2023-12-21 10:35 +0800
---

> 随缘更新

## 用过的招

### git 创建空分支

思路：`checkout`一个新的 orphan 分支，然后删除所有文件，添加一个文件，提交，推送。

```bash
git checkout --orphan emptyBranch
git rm -rf . //删除所有文件
echo '# new branch' >> README.md
git add README.md
git commit -m 'new branch'
git push origin emptyBranch
```

[参考链接：https://juejin.cn/post/6844904056436031496](https://juejin.cn/post/6844904056436031496)

### git 分支重命名

```bash
git checkout <branch> - 切换到目标分支
git branch -m <new_branch_name> - 重命名当前分支
git push origin -u <new_branch_name> - 推送分支
```

[参考链接](https://blog.csdn.net/Wustfish/article/details/131411472)

## 踩过的坑

### git 配置 ssh 之后 git push 仍然提示输入密码

{:.prompt-info}

> 破案：git clone 的时候采用了 https 协议。

查看远程仓库地址。

```bash
git config --get remote.origin.url
```

结果返回了一个 https 地址。

```text
https://github.com/minicoderwen/minicoderwen.github.io.git
```

`git remote` 操作的支持已经被移除了，不能用 https 操作而是 ssh。所以得重新设置远程仓库地址。

```bash
git remote set-url 《这里填仓库 ssh 的地址》
git push
```

[参考链接：https://www.zhihu.com/question/55865892](https://www.zhihu.com/question/55865892)

### git console 输出中文乱码

```bash
git config --global core.quotepath off
```

### git push 时出现 Connection closed

```console
Connection closed by 198.18.15.130 port 22
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

原因：机场关了 22 端口。用 https 协议 push，也就是 ssh 端口改成 443。
修改 `~/.ssh/config` 文件。

```bash
Host github.com
    HostName ssh.github.com
    User git
    Port 443
```

执行 `ssh -T git@github.com` 试一下。
