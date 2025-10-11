# Git 常用命令速查表

## 使用**Git将代码提交到GitHub**基本流程

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

---

## 规范化Git提交信息

### 下载

下载`Node.js`和`npm`

![image-20251011114917047](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20251011114917047.png)

![image-20251011130006721](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20251011130006721.png)

**注意：在bash而非cmd**

全局安装 Commitizen

```bash
npm install -g commitizen
```

初始化Commitizen配置

```bash
commitizen init cz-conventional-changelog --save-dev --save-exact
```

### 使用

安装并配置完成后，你可以使用 git cz 命令来替代 git commit，这将启动 Commitizen 的交互式提交信息填写流程。

```bash
git cz 或者  npx git-cz
```

按照提示，逐步填写提交信息的各个部分，如提交类型（feat、fix、docs 等）、提交范围、简短描述、详细描述、破坏性变更、关闭的问题等。完成后，Commitizen 将生成符合约定格式的提交信息。

在项目中安装并配置好 Commitizen 后，你可以使用 git cz 命令来替代 git commit，启动交互式提交信息填写流程。

### 示例

假设我们在一个项目中完成了一个新功能的开发，现在要提交这些更改。

1.在命令行中，进入项目根目录，运行以下命令：

```bash
npx git-cz
```

2.Commitizen 将启动交互式提交信息填写流程，首先会提示你选择提交类型：

> 选择一个提交类型:
>   feat:     新功能
>   fix:      修复 Bug
>   docs:     文档更新
>   style:    代码样式调整
>   refactor: 代码重构
>   perf:     性能优化
>   test:     添加或更新测试
>   chore:    构建过程或辅助工具的变动

根据我们的更改类型，选择 `feat` 并按下回车键。

![img](https://i-blog.csdnimg.cn/blog_migrate/c1acfe4caff1f93d82a0dff781074ff0.png)

3.接下来，Commitizen 会提示你输入提交的范围（可选）：

`这次提交的改动所影响的范围? (按回车键跳过)`

![img](https://i-blog.csdnimg.cn/blog_migrate/118e48c9083110fc0e105812c5519088.png)

如果我们的更改影响到特定的模块或组件，可以在这里输入相应的范围，否则直接按回车键跳过。

4.然后，Commitizen 会提示你输入一个简短的描述：

`写一个简短的变化描述，使用命令式语气，尽量包含主语（50个字符以内）:`
在这里，我们输入一个简洁明了的描述，说明这次提交的主要变化，例如：

`添加用户注册功能`

5.接下来，Commitizen 会提示你输入一个更详细的描述（可选）：

`提供一个更加详细的变化描述（按回车键跳过）。使用 "|" 换行:`
如果需要提供更多关于这次变更的信息，可以在这里输入多行描述，每行以 `|` 符号开头。如果不需要详细描述，直接按回车键跳过。

6.然后，Commitizen 会询问是否有任何破坏性变更：

`这次变化是否包含任何破坏性变更? (y/N)`
如果这次提交包含了破坏性变更，即可能影响到其他部分的功能或者与之前的版本不兼容，需要输入 y 并按回车键。否则，直接按回车键选择默认的 N。

7.最后，Commitizen 会询问这次提交是否关闭了某个 Issue：

`这次变化是否关闭了某个 Issue? (y/N)`
如果这次提交解决了某个 Issue，可以输入 y 并在提示中输入 Issue 的编号，多个 Issue 编号以逗号分隔。如果不关闭任何 Issue，直接按回车键选择默认的 N。

8.完成以上步骤后，Commitizen 会生成符合约定格式的提交信息，并显示出来供你确认：

`feat: 添加用户注册功能  
确认无误后，按回车键完成提交。`

![img](https://i-blog.csdnimg.cn/blog_migrate/c3b801f76467ffa431f53a4ff78aa619.png)

通过以上步骤，我们就使用 Commitizen 生成了一条规范化的提交信息，这样就可以在项目中保持一致的提交信息格式，方便后续的维护和追踪。

---

## 基本概念

| 名称   | 说明                 |
|--------|----------------------|
| master | 默认开发分支         |
| HEAD   | 当前分支的最新提交   |
| origin | 默认远程版本库       |

---

**Commit**操作是将更改从暂存区提交到本地仓库的过程。这是一个重要的步骤，因为它记录了代码的变更历史，允许开发者在未来回溯到任何一个提交点。每次执行*git commit*时，都会在本地仓库生成一个唯一的哈希值，称为**commit-id**，这个id在版本回退时非常有用。提交时通常会附带一条消息，用于描述这次提交的内容和目的，例如：

```bash
git commit -m "修复登录功能中的一个错误"
```

在提交之前，通常会使用*git add*命令将更改的文件从工作区添加到暂存区。*git add*可以接受不同的参数来指定要添加的文件范围，例如*git add .*会添加当前目录下所有更改的文件。

**Push**操作是将本地仓库的提交推送到远程仓库的过程。这一步是多人协作中同步代码的关键，它确保了团队成员可以共享他们的代码更改。执行*git push*时，你需要指定远程仓库的名称和要推送的分支，如：

```bash
git push origin master
```

这里，*origin*是远程仓库的名称，*master*是要推送的分支名称。如果本地分支与远程分支之间存在追踪关系，那么分支名称可以省略。

**区别和联系**

**Commit**和**Push**的主要区别在于它们作用的范围。**Commit**仅影响本地仓库，而**Push**则影响远程仓库。在日常开发中，开发者首先在本地进行代码更改，然后通过**commit**记录这些更改。当准备好与其他人分享这些更改时，他们会执行**push**操作，将本地的提交推送到远程仓库。

在推送之前，建议先从远程仓库拉取最新的代码，以避免潜在的冲突。如果在推送过程中遇到冲突，需要先解决冲突再进行推送。

---

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

2.中间要仔细看命令行提示，比如哪里点击e什么的

3.按操作来，别跳

---

## 解决Git分支分叉问题

![image-20251006182939598](C:\Users\HUAWEI\AppData\Roaming\Typora\typora-user-images\image-20251006182939598.png)









---

## 优雅地提交Git Commit Message

### Commit Message 格式

目前规范使用较多的是 [Angular 团队的规范](https://link.zhihu.com/?target=https%3A//github.com/angular/angular.js/blob/master/DEVELOPERS.md%23-git-commit-guidelines), 继而衍生了 [Conventional Commits specification](https://link.zhihu.com/?target=https%3A//conventionalcommits.org/). 很多工具也是基于此规范, 它的 message 格式如下:

```text
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

我们通过 git commit 命令带出的 vim 界面填写的最终结果应该类似如上这个结构, 大致分为三个部分(使用空行分割):

- 标题行: 必填, 描述主要修改类型和内容
- 主题内容: 描述为什么修改, 做了什么样的修改, 以及开发的思路等等
- 页脚注释: 放 Breaking Changes 或 Closed Issues

分别由如下部分构成:

- type: commit 的类型
- feat: 新特性
- fix: 修改问题
- refactor: 代码重构
- docs: 文档修改
- style: 代码格式修改, 注意不是 css 修改
- test: 测试用例修改
- chore: 其他修改, 比如构建流程, 依赖管理.
- scope: commit 影响的范围, 比如: route, component, utils, build...
- subject: commit 的概述, 建议符合 [50/72 formatting](https://link.zhihu.com/?target=https%3A//stackoverflow.com/questions/2290016/git-commit-messages-50-72-formatting)
- body: commit 具体修改内容, 可以分为多行, 建议符合 [50/72 formatting](https://link.zhihu.com/?target=https%3A//stackoverflow.com/questions/2290016/git-commit-messages-50-72-formatting)
- footer: 一些备注, 通常是 BREAKING CHANGE 或修复的 bug 的链接.

这样一个符合规范的 commit message, 就好像是一份邮件.

### git commit模版

只是个人的项目,或者想尝试一下这样的规范格式, 那么可以为 git 设置 commit template, 每次 git commit 的时候在 vim 中带出, 时刻提醒自己:

修改 ~/.gitconfig, 添加:

```text
[commit]
template = ~/.gitmessage
```

新建 ~/.gitmessage 内容可以如下:

```text
# head: <type>(<scope>): <subject>
# - type: feat, fix, docs, style, refactor, test, chore
# - scope: can be empty (eg. if the change is a global or difficult to assign to a single component)
# - subject: start with verb (such as 'change'), 50-character line
#
# body: 72-character wrapped. This should answer:
# * Why was this change necessary?
# * How does it address the problem?
# * Are there any side effects?
#
# footer: 
# - Include a link to the ticket, if any.
# - BREAKING CHANGE
#
```

### Commitizen: 替代你的 git commit

[commitizen/cz-cli](https://link.zhihu.com/?target=https%3A//github.com/commitizen/cz-cli), 我们需要借助它提供的 git cz 命令替代我们的 git commit 命令, 帮助我们生成符合规范的 commit message.

除此之外, 我们还需要为 commitizen 指定一个 Adapter 比如: [cz-conventional-changelog](https://link.zhihu.com/?target=https%3A//github.com/commitizen/cz-conventional-changelog) (一个符合 Angular团队规范的 preset). 使得 commitizen 按照我们指定的规范帮助我们生成 commit message.

#### 全局安装

```bash
npm install -g commitizen cz-conventional-changelog
echo '{ "path": "cz-conventional-changelog" }' > ~/.czrc
```

主要, 全局模式下, 需要 ~/.czrc 配置文件, 为 commitizen 指定 Adapter.

#### 项目级安装

```bash
npm install -D commitizen cz-conventional-changelog
```

package.json中配置:

```json
"script": {
    ...,
    "commit": "git-cz",
},
 "config": {
    "commitizen": {
      "path": "node_modules/cz-conventional-changelog"
    }
  }
```

如果全局安装过 commitizen, 那么在对应的项目中执行 git cz or npm run commit 都可以.

效果如下:

![img](https://pic4.zhimg.com/v2-b2176482b433c658b5687576f46e0b35_1440w.jpg)

### Commitlint: 校验你的 message

[commitlint](https://link.zhihu.com/?target=https%3A//github.com/marionebl/commitlint): 可以帮助我们 lint commit messages, 如果我们提交的不符合指向的规范, 直接拒绝提交, 比较狠.

同样的, 它也需要一份校验的配置, 这里推荐 [@commitlint/config-conventional](https://link.zhihu.com/?target=https%3A//github.com/marionebl/commitlint/tree/master/@commitlint/config-conventional) (符合 Angular团队规范).

安装:

```bash
npm i -D @commitlint/config-conventional @commitlint/cli
```

同时需要在项目目录下创建配置文件 .commitlintrc.js, 写入:

```js
module.exports = {
  extends: [
    ''@commitlint/config-conventional''
  ],
  rules: {
  }
};
```

### standard-version: 自动生成 CHANGELOG

通过以上工具的帮助, 我们的工程 commit message 应该是符合 Angular团队那套, 这样也便于我们借助 [standard-version](https://link.zhihu.com/?target=https%3A//github.com/conventional-changelog/standard-version) 这样的工具, 自动生成 CHANGELOG, 甚至是 语义化的版本号([Semantic Version](https://link.zhihu.com/?target=http%3A//semver.org/lang/zh-CN/)).

安装使用:

```bash
npm i -S standard-version
```

package.json 配置:

```json
"scirpt": {
    ...,
    "release": "standard-version"
}
```

PS: standard-version 有很多其他的特性, 这里不过多涉及, 有兴趣的同学自行尝试

----

#### 配置
Commitizen 的配置文件通常位于项目根目录下的 .czrc 或 package.json 中的 config.commitizen 字段。

示例配置：

```bash
{
  "path": "cz-conventional-changelog",
  "maxHeaderWidth": 100,
  "maxLineWidth": 100,
  "defaultType": "",
  "defaultScope": "",
  "defaultSubject": "",
  "defaultBody": "",
  "defaultIssues": "",
  "types": {
    "feat": {
      "description": "新功能",
      "title": "Features"
    },
    "fix": {
      "description": "Bug 修复",
      "title": "Bug Fixes"
    },
    // ...
  }
}
```

可以根据项目需求自定义提交类型、默认值等配置项。

#### 插件开发
Commitizen 支持自定义适配器插件，以满足不同项目的提交信息格式要求。你可以开发自己的适配器插件，或者使用社区提供的插件。

一个简单的适配器插件示例：

```bash
const conventionalCommitTypes = require('conventional-commit-types');

module.exports = {
  prompter(cz, commit) {
    cz.prompt([
      {
        type: 'list',
        name: 'type',
        message: '选择提交类型:',
        choices: conventionalCommitTypes.types,
      },
      {
        type: 'input',
        name: 'subject',
        message: '简短描述:',
        validate: (input) => input.length > 0,
      },
      // ...
    ]).then((answers) => {
      const message = `${answers.type}: ${answers.subject}`;
      commit(message);
    });
  },
};
```

插件需要导出一个 prompter 函数，接收 cz 和 commit 两个参数。使用 cz.prompt 方法定义交互式问题，收集用户输入，最后调用 commit 函数生成最终的提交信息。

#### 集成
Commitizen 可以与其他工具和流程集成，如 Git 钩子、持续集成等。

例如，你可以在 Git 的 pre-commit 钩子中检查提交信息是否符合 Commitizen 的格式要求：

```bash
#!/bin/sh

# 检查是否存在未暂存的更改
if ! git diff --quiet HEAD; then
  echo "存在未暂存的更改，请先提交或暂存这些更改。"
  exit 1
fi

# 运行 Commitizen
exec < /dev/tty && node_modules/.bin/git-cz --hook || true
```

这样，在每次提交前，都会自动启动 Commitizen 的交互式提交信息填写流程。

---

## 参考：

[(71 封私信 / 80 条消息) 优雅的提交你的 Git Commit Message - 知乎](https://zhuanlan.zhihu.com/p/34223150)

https://blog.csdn.net/black_sneak/article/details/139600633

[(71 封私信 / 80 条消息) git 提交后怎么修改commit的描述信息？ - 知乎](https://zhuanlan.zhihu.com/p/7035085243)

[Git使用小技巧【修改commit注释, 超详细】_git更改commit描述-CSDN博客](https://blog.csdn.net/xiaoyulike/article/details/119176756)

[Commitizen：规范化你的 Git 提交信息-CSDN博客](https://blog.csdn.net/imdeity/article/details/137377949)

[windows安装npm教程_npm 安装-CSDN博客](https://blog.csdn.net/zhouyan8603/article/details/109039732)



感谢！