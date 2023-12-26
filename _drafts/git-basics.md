---
layout: post
title: 用命令行驾驭git
description: git basics
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

## 1. Git 101

### 1.1. Git最基础的操作

| 初始化一个新的`repository` |
| -------------------------- |
| `git init`                 |

| 添加文件到`repository`    |
| ------------------------- |
| `git add .`               |
| `git commit -m "message"` |

| 获取`commit`历史 |
| ---------------- |
| `git log`        |

返回结果如下：

```console
commit 9f1944464c12e9ff09a9b1f32245bfc705e8732d (HEAD -> main)
Author: minicoderwen <373441770@qq.com>
Date:   Fri Dec 22 00:28:11 2023 +0000

    update karabiner # 这个是commit message
```

然后就可以用获取到的`commit id`来

| 切换到某个`commit`                                      |
| ------------------------------------------------------- |
| `git checkout 9f1944464c12e9ff09a9b1f32245bfc705e8732d` |

### 1.2. Git的`branch`

| 查看所有`branch` |
| ---------------- |
| `git branch`     |

| 创建`branch`的几种方式       |
| ---------------------------- |
| `git branch new_branch`      |
| `git checkout new_branch`    |
| `git checkout -b new_branch` |
| `git switch -c new_branch`   |

| 合并`branch`           |
| ---------------------- |
| `git merge new_branch` |

| 切换`branch`                 |
| ---------------------------- |
| `git switch existing_branch` |

| 查看`staging area`中的所有文件 |
| ------------------------------ |
| `git ls-files`                 |

### 1.3. Git Undo

#### 1.3.1. 如何删除已经`staged`的文件？

首先，删除文件本身，再

| 告诉git我们要删除这个文件 |
| ------------------------- |
| `git rm some-file`        |
| `git add .`               |

这两条命令实现同样的效果，那就是告诉git不要再追踪这个文件了。

#### 1.3.2. 如何撤销对`unstaged`文件的修改？

假设我们修改了`some-file`，现在我们想要撤销修改。我们可以`git checkout some-file`让他回退到上一个`commit`的初始状态。

那么如何撤销对所有文件的修改呢？

| 撤销所有修改     |
| ---------------- |
| `git checkout .` |

这条命令表示回退当前分支下所有被追踪的文件到上一个`commit`的状态。`checkout`是git常用撤销修改的命令。但是git提供了一个更明确的指令，叫作`restore`，能实现同样的效果。

| 撤销修改                |
| ----------------------- |
| `git restore some-file` |
| `git restore .`         |

#### 1.3.3. 如何删除`untracked`的文件？

假设我们在项目中新增了一个文件`some-file`，在`git add some-file`之前这个文件是处于`untracked`状态的。如果我们想要删除这个文件，我们可以用`git clean`命令。

- `-d`表示删除目录
- `-f`表示强制删除
- `-n`表示列举出要删除的文件，但是不真的删除

| 删除untracked文件和目录     |
| --------------------------- |
| `git clean -d -f some-file` |

| 列举出untracked的文件和目录 |
| --------------------------- |
| `git clean -d -n`           |

#### 1.3.4. 如何撤销对`staged`文件的修改？

`git checkout`对已经`staged`的文件不生效了。

| 旧方法                |
| --------------------- |
| `git reset some-file` |

`git reset`会把`some-file`最新一次`commit`的内容覆盖到`staging area`，这个命令可以撤销对`staged`文件的修改，但是这个命令会把`staged`的文件变成`unstaged`的文件。如果我们不想要这个副作用，我们可以用`git restore`命令。

| 新方法                           |
| -------------------------------- |
| `git restore --staged some-file` |

#### 1.3.5. 如何删除本地的`commit`？

| 回退1个commit      |
| ------------------ |
| `get reset HEAD~1` |

| 回退2个commit      |
| ------------------ |
| `git reset HEAD~2` |

- 使用 `--soft`
  - --soft 表示回退到某个`commit`，但是保留所有修改在`staging area`，只有commit被回退了，可以直接再次`git commit`。
  - 不用 --soft 表示修改的内容会被从`staging area`移除到`unstaged`，但是修改不会被删除。
- 使用 `--hard`

  - --hard 表示删除某一个（几个）`commit`并且删除所有的修改。

#### 1.3.6. 如何删除`branch`

| 使用 -d 删除branch          |
| --------------------------- |
| `git branch -d some_branch` |

| 使用 -D 删除branch          |
| --------------------------- |
| `git branch -D some_branch` |

- `-d` 表示删除一个已经`merge`的`branch`，如果`branch`没有被`merge`，则会报错。
- `-D` 表示强制删除一个`branch`，不管`branch`有没有被`merge`。

### 1.4. Git中的`detached HEAD`

当我们`checkout`某一个特定的`commit`，这时候就处于`detached`的状态，表示`HEAD`指向的不是一个`branch`，而是一个`commit`。有时候我们在`commit`的时候忘了一些东西，我们可以用这种方法来切换到那个`commit`特定的节点，去补充我们落下的东西。

那么如何在`detached HEAD`模式下进行修改并合并进`main`目标分支呢？

```bash
# 首先checkout某一个commit
git checkout 9f1944464c12e9ff09a9b1f32245bfc705e8732d
# 进行修改，然后commit
git add some-file
git add .
git commit -m "some message"
```

此时`main`分支是不包含这个`commit`的，我们需要新建一个分支包含这个`commit`，再合并到`main`目标分支。

```bash
git log # 获取我们要合并的commit id，类似 037e3f3
git branch new_branch 037e3f3 # 新建一个分支包含这个commit
git branch # 此时查看分支，会发现 detached HEAD的提示没有了
git checkout main # 切换到main分支
git merge new_branch # 合并new_branch分支
git branch -d new_branch # 删除new_branch分支，因为已经不需要这个分支了
```

### 1.5. 什么是.gitignore？

我们可以创建一个`.gitignore`来告诉git哪些文件不需要被追踪，比如IDE的配置文件、编译后的文件、日志文件等。

```plaintext
*.log # 忽略所有的log文件
!test.log # 但是不忽略test.log

some_folder/ # 忽略some_folder目录下的所有文件
**/some_folder/ # 忽略所有目录下的some_folder目录
**/some_folder/** # 忽略所有目录下的some_folder目录和文件
```

### 1.6. Git基础命令总结

| 命令                           | 作用                                          |
| ------------------------------ | --------------------------------------------- |
| git --version                  | 查看git版本                                   |
| git init                       | 初始化一个新的repository                      |
| git status                     | 查看当前状态                                  |
| git log                        | 查看commit历史                                |
| git ls-files                   | 查看staging area中的所有文件                  |
| git add some-file              | 添加文件到staging area                        |
| git add .                      | 添加所有文件到staging area                    |
| git commit -m "message"        | 提交staging area中的文件到repository          |
| git checkout commitid          | 切换到某个commit(detached HEAD)               |
| git branch branch_name         | 新建一个branch                                |
| git checkout branch_name       | 切换到某个branch                              |
| git checkout -b branch_name    | 新建一个branch并切换到这个branch              |
| git switch -c branch_name      | 新建一个branch并切换到这个branch              |
| git merge other_branch         | 合并other_branch到当前branch                  |
| git rm some-file               | 删除文件并且告诉git不要再追踪这个文件         |
| git restore some-file          | 撤销对unstaged文件的修改                      |
| git restore --staged some-file | 撤销对staged文件的修改                        |
| git clean -df some-file        | 删除untracked文件                             |
| git reset HEAD~1               | 回退1个commit                                 |
| git reset --soft HEAD~1        | 回退1个commit，但是保留所有修改在staging area |
| git reset --hard HEAD~1        | 回退1个commit，删除所有修改                   |
| git branch -D branch_name      | 强制删除branch                                |
