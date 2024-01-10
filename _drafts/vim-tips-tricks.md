---
layout: post
title: VIM 使用技巧
description: A collection of vim tips and tricks.
image: https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401100835492.jpg
category:
  - 不学无术
tags:
  - vim
toc: true
comments: true
math: false
mermaid: false
pin: false
sitemap: true
published: true
lang: zh-CN
---

> 随缘更新

#### 比较文件差异

1. 方式一：使用`vim/nvim -d file1 file2`
2. 方式二：如果已经打开了 vim/nvim，先垂直分屏（vertical split)，然后执行`:windo diffthis`

#### 删除包含`sum`的行

`g/sum/d`

#### 删除不包含`sum`的行

`g!/sum/d`
