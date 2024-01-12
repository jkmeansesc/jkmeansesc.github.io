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

## Git 101

### Git基础操作

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

### Branch 分支

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

### Git Undo 撤销

#### 如何删除已经 `staged` 的文件？

首先，删除文件本身，再

| 告诉git我们要删除这个文件 |
| ------------------------- |
| `git rm some-file`        |
| `git add .`               |

这两条命令实现同样的效果，那就是告诉git不要再追踪这个文件了。

#### 如何撤销对 `unstaged` 文件的修改？

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

#### 如何删除 `untracked` 的文件？

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

#### 如何撤销对 `staged` 文件的修改？

`git checkout`对已经`staged`的文件不生效了。

| 旧方法                |
| --------------------- |
| `git reset some-file` |

`git reset`会把`some-file`最新一次`commit`的内容覆盖到`staging area`，这个命令可以撤销对`staged`文件的修改，但是这个命令会把`staged`的文件变成`unstaged`的文件。如果我们不想要这个副作用，我们可以用`git restore`命令。

| 新方法                           |
| -------------------------------- |
| `git restore --staged some-file` |

#### 如何删除本地的 `commit`？

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

#### 如何删除 `branch`

| 使用 -d 删除branch          |
| --------------------------- |
| `git branch -d some_branch` |

| 使用 -D 删除branch          |
| --------------------------- |
| `git branch -D some_branch` |

- `-d` 表示删除一个已经`merge`的`branch`，如果`branch`没有被`merge`，则会报错。
- `-D` 表示强制删除一个`branch`，不管`branch`有没有被`merge`。

### 什么是detached HEAD

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

### 什么是.gitignore？

我们可以创建一个`.gitignore`来告诉git哪些文件不需要被追踪，比如IDE的配置文件、编译后的文件、日志文件等。

```plaintext
*.log # 忽略所有的log文件
!test.log # 但是不忽略test.log

some_folder/ # 忽略some_folder目录下的所有文件
**/some_folder/ # 忽略所有目录下的some_folder目录
**/some_folder/** # 忽略所有目录下的some_folder目录和文件
```

### Git基础命令总结

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

## 什么是Git Stash

如果我们在`branch1`上修改了一些文件，但是我们想要切换到`branch2`上去，但是我们又不想要把修改的内容`commit`到`branch1`上，这时候我们可以用`git stash`命令。`git stash`会将已经修改的内容存到栈中，然后回退到修改前`HEAD`的状态，我们可以在后续应用这些修改。

假设我们有一个文件`file.txt`，内容如下：

```plaintext
This is the original content.
```

修改`file.txt`，内容如下：

```plaintext
This is the original content.
This is added.
```

### git stash

执行`git stash`，然后我们再查看`file.txt`，会发现文件恢复到了修改前的状态。

```plaintext
This is the original content.
```

再次修改`file.txt`，内容如下：

```plaintext
This is the original content.
This is added again.
```

### git stash list

再次执行`git stash`，此时栈中会有两个版本的`stash`。我们可以使用`git stash list`查看栈中的内容。

```plaintext
stash@{0}: WIP on main: aa2183d commit name
stash@{1}: WIP on main: aa2183d commit name
```

### git stash apply

如果想要应用`stash`中的修改，我们可以用`git stash apply`命令，这个命令会将栈顶（也就是`stash@{0}`）的修改应用到当前分支。

> `git stash apply`永远应用的是最新的`stash`，所以在这里`This is added again.`会被应用而不是`This is added.`

如果我们想要应用指定的`stash`，执行`git stash apply 1`。此时，`This is added.`会被应用。

### git stash push

我们可以用`git stash push -m "some message"`来记录某一个`stash`的信息。此时执行`git stash list`，`stash@{0}`会显示

```plaintext
stash@{0}: On main: some message
```

这个方法可以让我们更好地区分不同的`stash`。

### git stash pop

当我们决定好要应用哪一个`stash`后，我们可以使用`git stash pop`，这个命令会将栈顶的`stash`应用到当前分支，并且将栈顶的`stash`删除。此时之前的`stash@{1}`会变成`stash@{0}`。

### git stash drop

我们可以用`git stash drop`来删除某一个`stash`，比如`git stash drop 1`。

### git stash clear

我们可以用`git stash clear`来清空栈中所有的`stash`。

## 什么是Git Reflog

`git reflog`可以查看所有的`commit`历史，包括`reset`和`rebase`的历史。我们可以用这个命令来找回丢失的`commit`和`branch`。

```console
aa2183d (HEAD -> main, origin/main, origin/HEAD) HEAD@{0}: reset: moving to HEAD
aa2183d (HEAD -> main, origin/main, origin/HEAD) HEAD@{1}: reset: moving to HEAD
aa2183d (HEAD -> main, origin/main, origin/HEAD) HEAD@{2}: commit: nvim add trouble
60d9e5b HEAD@{3}: commit: update nvim
34fb74a HEAD@{4}: commit: update nvim
4b97fc3 HEAD@{5}: commit: update zshrc
f1259a9 HEAD@{6}: commit: update nvim
```

使用`git reflog`找回丢失的commit。

```bash
git reflog
git reset --hard 60d9e5b
git log # 此时60d9e5b又变成了HEAD
```

使用`git reflog`找回丢失的branch。

```bash
git checkout -b new_branch
git add .
git commit -m "file added"
git switch main
git branch -D new_branch # 删除分支
git reflog # 找到 "file added" 这个commit id
git checkout 60d9e5b # 此时处于detached模式
git switch -c new_branch # 创建一个包含这个commit的新分支
```

## 什么是Git Merge

假设有一个`new_branch`分支需要合并到`master`分支：

- 如果`new_branch`有新的`commit`，`master`没有，`git merge`会默认使用`fast-forward`进行合并。
- 如果`new_branch`和`master`都有新的`commit`，`git merge`会默认使用`non-fast-forward recursive merge`进行合并。

### 什么是fast-forward

```bash
git checkout -b new_branch # 从master创建一个新分支
# 在新分支上修改新增内容并提交
git checkout master # 切换到master分支
git merge new_branch # 将new_branch合并到master
```

从`master`上创建新分支并进行开发，之后新分支的提交合并回`master`分支，此时`fast-forward`发生。

`fast-forward`不会产生新的`commit`，只是将`master`指向了`new_branch`的`commit`。

```console
commit dc266f220355690a48a56b520dbdd0aeb2a8d4e8 (HEAD -> main, origin/main, origin/HEAD)
Author: minicoderwen <373441770@qq.com>
Date:   Thu Jan 4 11:58:23 2024 +0000

    new_branch commit 2 <<<<<<<<<<< fast-forward merge之后，新的HEAD指向这个new_branch的最后一次commit

commit f2a4922b8d01b2365e6056bee765701ad063adbe
Author: minicoderwen <373441770@qq.com>
Date:   Sat Dec 30 05:31:36 2023 +0000

    new_branch commit 1

commit 3d6ff634564a0d17a0f3cd83d2f8e5c4acf86ff5
Author: BigWen <48466898+minicoderwen@users.noreply.github.com>
Date:   Thu Dec 28 07:24:21 2023 +0000

    original master commit <<<<<<<<<<< 原来的master分支指向这个分支
```

#### 了解 --squash

我们可以执行`git reset --hard HEAD~2`先回退`master`分支到`original master commit`。

```bash
git merge --squash new_branch
```

`--squash` 会将`new_branch`的所有`commit`合并成一个`commit`，然后将这个`commit`应用到`master`分支上。

此时执行`git log`

```console
commit 3d6ff634564a0d17a0f3cd83d2f8e5c4acf86ff5 (HEAD -> main, origin/main, origin/HEAD)
Author: BigWen <48466898+minicoderwen@users.noreply.github.com>
Date:   Thu Dec 28 07:24:21 2023 +0000

    original master commit
```

此时并没有新的分支被创建，但是`new_branch`上的所有改动已经被添加到了`staging area`。

最后执行`git commit -m "some message"`，`new_branch`上的所有改动就被合并到了`master`上。

### 什么是non-fast-forward recursive merge

#### 了解 --no-ff

`--no-ff`表示不使用`fast-forward`进行合并。

```bash
git merge --no-ff new_branch
```

`--no-ff`的应用场景：`new_branch`创建之后，`master`有了新的`commit`，`new_branch`也有了新的`commit`，此时这两个分支的`base`已经不一样了。

![git branch](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401101016291.png)

此时使用`--no-ff`进行合并，会产生一个新的`commit`，这会将`master`和`new_branch`上的所有修改合并成一个`commit`。

此时执行`git log`，`new_branch`上的分支已经被合并到`master`了。

如果要回退，只需要`git reset --hard HEAD~1`（不需要数`new_branch`的`commit`数量）。此时`new_branch`上的分支也会被从`master`上回退。

## 什么是Git Rebase

![git rebase](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401101948187.png)

如图所示，`git rebase`可以将`feature`分支中的`f.1`和`f.2`这两个`commit`添加到已经新增了`m.3`这个`commit`的`master`分支上。这个属于`fast-forward merge`。

原理解析：

![git rebase](https://raw.githubusercontent.com/minicoderwen/picwen/main/img/202401101954375.png)

- "m.3"变成`feature`分支的`base`。
- `master`分支`rebase`到`feature`分支。
- 合并`feature`分支到`master`分支。

> `rebase`并不会移动`commit`，而是创建新的`commit`。所以不要`rebase`到不是你的分支上。

```bash
git checkout feature
git rebase master
git checkout master
git merge feature
```

### 什么时候使用rebase？

- 当`feature`分支还没合入`master`但`master`已经有了新的`commits`。

> 注意：`rebase`会更改代码的历史，因为`rebase`后`commit id`变了。

## 如何处理冲突？

查看这篇博文。
