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

---

https://blog.csdn.net/black_sneak/article/details/139600633