# Git 常用命令速查表

### 使用**Git将代码提交到GitHub**基本流程

1.克隆仓库

在github克隆库（code-ssh)，得到代码地址

2.选定文件存储地方，右键`Git Bash Here`

3.在终端（git bash）输入

```bash
$ git clone <git复制>
```

4.点开此文件夹，可以开始改动。之后右键进入终端

```bash
$ git add <file>
$ git commit -m " "
$ git push origin main
```

## 基本概念

| 名称   | 说明                 |
|--------|----------------------|
| master | 默认开发分支         |
| HEAD   | 当前分支的最新提交   |
| origin | 默认远程版本库       |

---

## 创建版本库
```bash
$ git init              # 初始化本地版本库
$ git clone <url>       # 克隆远程版本库
```

---

## 分支与标签
```bash
$ git branch                # 显示所有本地分支
$ git branch <new-branch>   # 创建新分支
$ git branch -d <branch>    # 删除本地分支
$ git checkout <branch/tag> # 切换到指定分支或标签
$ git tag                   # 列出所有本地标签
$ git tag <tagname>         # 基于最新提交创建标签
$ git tag -d <tagname>      # 删除标签
```

---

## 修改和提交
```bash
$ git status                # 查看状态
$ git diff                  # 查看变更内容
$ git add                   # 跟踪所有改动过的文件
$ git add <file>            # 跟踪指定的文件
$ git mv <old> <new>        # 文件改名
$ git rm <file>             # 删除文件
$ git rm --cached <file>    # 停止跟踪文件但不删除
$ git commit -m "message"   # 提交所有更新过的文件
$ git commit --amend        # 修改最后一次提交
```

---

## 合并与衍合
```bash
$ git merge <branch>    # 合并指定分支到当前分支
$ git rebase <branch>   # 衍合指定分支到当前分支
```

---

## 远程操作
```bash
$ git remote -v                 # 查看远程版本库信息
$ git remote show <remote>      # 查看指定远程版本库信息
$ git remote add <remote> <url> # 添加远程版本库
$ git fetch <remote>            # 从远程库获取代码
$ git pull <remote> <branch>    # 下载代码并快速合并
$ git push <remote> <branch>    # 上传代码并快速合并
$ git push <remote> :<branch>   # 删除远程分支或标签
$ git push --tags               # 上传所有标签
```

---

## 查看提交历史
```bash
$ git log               # 查看提交历史
$ git log -p <file>     # 查看指定文件的提交历史
$ git blame <file>      # 以列表方式查看指定文件的提交历史
```

---

## 撤销
```bash
$ git reset --hard HEAD     # 撤销工作目录中所有未提交文件的修改
$ git checkout HEAD <file>  # 撤销指定未提交文件的修改
```
## 提交

push：该单词直译过来就是 “推” 的意思，如果我们本地的代码有了更新，为了保持本地与远程的代码同步，我们就需要把本地的代码推到远程的仓库，代码示例：

```bash
git push origin master
```

pull：该单词直译过来就是 “拉” 的意思，如果我们远程仓库的代码有了更新，同样为了保持本地与远程的代码同步，我们就需要把远程的代码拉到本地，代码示例：

```bash
git pull origin master
```

## 修改之前的commit备注

### 1、修改还没push的commit信息

当我们只是想要修改最近一次提交的描述信息，且这个提交尚未被推送到远程仓库时，可以使用`git commit --amend`命令。

#### （1）查看提交历史

在修改之前，我们可以通过`git log`命令来查看提交历史。`git log`会以列表的形式显示每个提交的哈希值、作者、日期以及提交信息等内容。这一步主要是为了确认我们要修改的是最近一次提交。

![img](https://pic4.zhimg.com/v2-628f2f26411d47203439fa58ba6ee3a1_1440w.jpg)

这里可以看到我们最新的一次提交信息写的是`提交test文件111`

#### （2）执行修改命令

运行`git commit --amend`命令。执行该命令后，Git会打开一个文本编辑器（通常是系统默认的文本编辑器，如`vim`或`nano`），在这个编辑器中会显示出原来的提交信息。我们可以在这个编辑器中对提交信息进行修改。

![img](https://pic1.zhimg.com/v2-2f0ef4a5399f4ae16526d6f865f33492_1440w.jpg)

#### （3）保存并完成修改

完成信息修改后，保存并关闭编辑器。此时，Git就会用新的描述信息覆盖原来的最近一次提交的描述信息。

![img](https://picx.zhimg.com/v2-0a2befebc1361dd3c5c6354572ec47d5_1440w.jpg)

这样就将我们的提交信息修改成`提交test文件`了。

### 2、修改历史提交的描述信息

如果我们想要修改的是历史提交的描述信息，尤其是在已经有多次提交或者提交已经被推送到远程仓库的情况下，情况会稍微复杂一些，我们可以使用[交互式变基](https://zhida.zhihu.com/search?content_id=250429541&content_type=Article&match_order=1&q=交互式变基&zhida_source=entity)（`git rebase -i`）的方法。但需要注意的是，这种方法会改写提交历史，因此如果已经将这些提交推送到远程仓库并且其他团队成员也在使用这个仓库，可能会引发一些问题。

#### （1）确定提交范围

首先，使用`git log`来确定要修改的提交的范围。我们需要找到要修改的提交的哈希值或者相对位置（例如，相对于当前分支头的第几个提交）。

![img](https://pic2.zhimg.com/v2-edc9d3a6dbea2438aef1ef96d72345fd_1440w.jpg)

#### （2）启动交互式变基

运行`git rebase -i [startpoint]`命令。这里的`[startpoint]`可以是一个提交的哈希值或者像`HEAD~n`这样的表示相对位置的表达式（`n`表示从当前分支头开始往前数的提交个数）。例如，如果我们想修改最近2个提交的信息，可以运行`git rebase -i HEAD~2`。

![img](https://pica.zhimg.com/v2-9c5edb783c39c5bee9bcb5799768f548_1440w.jpg)

#### （3）标记要修改的提交

执行上述命令后，Git会打开一个文本编辑器，里面列出了我们指定范围内的提交，每行一个提交，格式类似于`pick [commit - hash] [commit - message]`。我们需要将想要修改的提交那一行的`pick`改为`edit`。

![img](https://picx.zhimg.com/v2-be67912e1697db2f2562aff055b501e3_1440w.jpg)

我们将第一个提交的`pick`改为`edit`。

#### （4）暂停变基并修改信息

保存并关闭编辑器后，Git会开始交互式变基操作。当操作到我们标记为`edit`的提交时，Git会暂停变基过程。此时，我们可以运行`git commit --amend`命令来修改提交信息，操作方法和修改最近一次提交信息相同。

- **暂停变基的时候执行命令修改提交信息：**

```text
git commit --amend
```

![img](https://pic4.zhimg.com/v2-e25c4ab44c5af595447c9762a1a6811b_1440w.jpg)

![img](https://picx.zhimg.com/v2-b89a7babba2fac1d3f308d06518e74bb_1440w.jpg)

- **修改完成后继续变基操作，执行命令:**

```text
git rebase --continue
```

![img](https://pic3.zhimg.com/v2-862e9e8bc7e1b1e870aba145de0459bc_1440w.jpg)

这个时候再看一下提交记录，就会发现第二条提交记录的提交信息已经从“`提交test文件`”修改为“`提交test文件1`”了。

![img](https://pic1.zhimg.com/v2-4e2e54bfe57376d824bec9468bc9e03a_1440w.jpg)

### 3、注意

在修改已经推送到远程仓库的提交信息时，我们需要格外谨慎。如果其他团队成员已经拉取了旧的提交，我们修改并推送后，他们在拉取新的修改时可能会遇到[合并冲突](https://zhida.zhihu.com/search?content_id=250429541&content_type=Article&match_order=1&q=合并冲突&zhida_source=entity)等问题。所以，对于共享的远程仓库，最好在整个团队成员都了解情况并且同意的情况下，再对已经推送的提交历史进行修改。这样可以避免给团队协作带来不必要的麻烦，确保项目的版本控制和开发流程顺利进行。

注意：

1.**英文**状态下  i为输入，Esc为退出编辑，:wq为保存

2.这里点e![e0049b67d8289ab89a277f42d30b1cdf](E:\WeChat__\xwechat_files\wxid_al8al54za2ms22_d8bb\temp\RWTemp\2025-10\cd567f8c237e8ed331e9aacbe3614d01\e0049b67d8289ab89a277f42d30b1cdf.png)

3.按操作来，别跳

---

## 解决Git分支分叉问题

![image-20251006182939598](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20251006182939598.png)

https://blog.csdn.net/black_sneak/article/details/139600633