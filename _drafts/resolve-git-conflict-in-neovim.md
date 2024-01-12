---
layout: post
title: Neovim 处理 Git 冲突
description: This blog shows how to resolve git conflict in neovim using vim-fugitive.
image: https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202312201913798.jpg
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

### 在neovim中使用vim-fugitive插件解决git冲突

> 插件地址: [vim-fugitive](https://github.com/tpope/vim-fugitive)

`Gvdiffsplit!`命令可以在垂直分割窗口中打开当前文件的git diff，如下图

![git-conflict](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401121920662.png)

- 左边是`target branch`，也就是`main`
- 右边是`source branch`，也就是`deleteme`
- 中间，是当前文件的`working copy`

有两种方式解决冲突：`diffget`和`diffput`

- `diffget`，光标移至`working copy`冲突处，将左或右的内容拉入到中间。

  `diffget fugitive:///Users/oneoldmac/git/minicoderwen.github.io/.git//2/README.md`

  `diffget fugitive:///Users/oneoldmac/git/minicoderwen.github.io/.git//3/README.md`

  > 注意上面两条命令的路径名， ..//2/.. 永远是从`target branch`（左边）并入中间，..//3/.. 永远是从`source branch` （右边）并入中间

- `diffput`，光标移至左边，或者右边，将该分支内容`put`到中间。

  `diffput README.md`

最后，`Gwrite`保存，`Gcommit`提交。
