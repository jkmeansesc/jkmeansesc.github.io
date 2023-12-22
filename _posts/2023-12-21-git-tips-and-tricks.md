---
layout: post
title: git用过的招和踩过的坑
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

思路：`checkout`一个新的orphan分支，然后删除所有文件，添加一个文件，提交，推送。

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

结果返回了一个https地址。

```text
https://github.com/minicoderwen/minicoderwen.github.io.git
```

`git remote` 操作的支持已经被移除了，不能用 https 操作而是 ssh。所以得重新设置远程仓库地址。

```bash
git remote set-url <这里填仓库ssh的地址>
git push
```

[参考链接：https://www.zhihu.com/question/55865892](https://www.zhihu.com/question/55865892)

### git console 输出中文乱码

```bash
git config --global core.quotepath off
```

{% include busuanzi.html %}
