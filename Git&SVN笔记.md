# Git & SVN 使用指南

## 资源链接

- **[Git 下载地址](https://git-for-windows.github.io/)**  
  提供Git的Windows版本下载，适合在Windows系统上安装和使用Git。

- **[Git 中文手册](https://git-scm.com/book/zh/v2)**  
  Git的官方中文手册，详细介绍了Git的使用方法和最佳实践。


```cmd
$ git --help     //不知道怎么办就看帮助呗
```

## 时光穿梭机
### 版本退回【重要】
```
git reset --hard commit_id
```

* `HEAD` 指向的版本就是当前版本，因此，Git 允许我们在版本的历史之间穿梭，使用命令 `git reset --hard commit_id`
    * Git 的 `commit_d`(版本号)是一个十六进制的用 SHA1 计算出来的数字
    * 在 Git 中用 `HEAD` 表示当前版本, 上一个版本就是 `HEAD^`, 上一百个版本写成 `HEAD~100`
    * 使用 `git reset --hard HEAD^` 命令退回上一个版本
* 穿梭前，用 `git log` 可以查看提交历史，以便确定要回退到哪个版本
    * 【用这个】使用 `git log --pretty=oneline` 让记录单行显示
    * 使用 `git log` 或 `git reflog` 加 `file_name` 查看指定文件的历史
* 要重返未来，用 `git reflog` 查看命令历史，以便确定要回到未来的哪个版本
    * Git 提供一个命令 `git reflog` 来记录你的每一次命令, 这样就可以找到所有版本的 `commit id`  
***
### 工作区和暂存区
* **工作区(Working Directory)**  
你在电脑里能看到的目录
* **版本库(Repository)**  
工作区有一个隐藏目录 `.git`, 这个不算工作区, 而是 Git 的版本库.我们可以称它为 *Repo*  
*Repo* 里存放了很多东西, 其中最重要的就是暂存区 `stage`(或者叫 index), 还有 Git 为我们自动创建的第一个分支(`Branch`)`master`.以及指向 `master` 的一个指针叫 `HEAD`.  
![repo.png](img/repo.png)  
`cat file_name` 命令，其功能是显示在工作区、暂存区和分支里同名文档的 **最新修改版本的内容**

前面讲了我们把文件往 Git 版本库里添加的时候，是分两步执行的：  
第一步是用 `git add` 把文件添加进去，实际上就是把文件修改添加到暂存区；  
第二步是用 `git commit` 提交更改，实际上就是把暂存区的所有内容提交到当前分支。

#### 掌握仓库当前的状态 ` git status`
#### 查看修改内容
下面是关于 `git diff` 的一些使用区别    
另外可以使用 `git diff commit_id_1 commit_id_2` 比较两个不同版本的区别  

`cat file_name` 命令，其功能是显示在工作区、暂存区和分支里同名文档的 **最新修改版本的内容**


### 撤销修改【重要】
* **场景 1**：当你改乱了 **工作区** 某个文件的内容，想直接丢弃工作区的修改时，用命令 `git checkout -- file_name`
    * 可以用 `git checkout -- *` 丢弃所有工作区文件的修改
* **场景 2**：当你不但改乱了工作区某个文件的内容，还添加到了 **暂存区** 时，想丢弃修改，分两步
    * 第一步用命令 `git reset HEAD file_name` 就回到了 **场景 1**
        * 使用 `git reset HEAD` 丢弃所有暂存区的修改
    * 第二步按 **场景 1** 操作
* **场景 3**：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考 **版本回退** 一节，不过前提是没有推送到远程库。

注意: 使用版本退回操作 `git reset --hard ` 会导致所有暂存区和工作区的当前修改但未 commit 的内容全部丢弃.
特别: 使用 `git reset --hard HEAD` 会导致上述结果, 并在 `git reflog` 中生成记录, 但不改变


### 文件名里有空格和()的情况
Git 命令（或者所有的 shell 命令中）中出现括"（）"时系统把它看做一个有特殊意义的命令从而尝试去执行它。  
因此如果文件名中出现括号，系统找不到括号里要执行的命令就会报错。  
我们要让系统忽略括号的特殊意义，方法是用"双引号把文件名括起来，或者用转义符将括号转义.  
例如文件名为 git (1).md  
以下的写法都是可以的:  
`git add "git (1).md"`
`git add git" "\(1\).md`
`git add git" (1)".md`

### 删除文件
使用 `rm file_name` 删除本地文件  
使用 ` git rm file_name` 提交删除到暂存区  
使用 `git commit -m "balabala"` 提交到本地库    
**注意:** 可以直接使用 `git rm file_name` 删除本地文件以及提交删除到暂存区, 但仅用于暂存区有此本地文件的情况  

## 远程仓库

[简书-git ssh 免密登录](https://www.jianshu.com/p/7164a52786f3)

注册 GitHub 账号

由于你的本地 Git 仓库和 GitHub 仓库之间的传输是通过 SSH 加密的，所以，需要一点设置：

* 第 1 步：创建 SSH Key
```
$ ssh-keygen -t rsa -C  "youremail@example.com"
````
然后在用户主目录里找到 `.ssh` 目录, 里面有 `id_rsa` 和 `id_rsa.pub` 两个文件 `id_rsa` 是私钥, `id_rsa.pub` 是公钥.  

>`cd ~` 进入用户主目录
> `cd .ssh` 进入.ssh 目录
> `ls` 列出.ssh 目录的文件
> `cat ~/.ssh/id_rsa.pub` 看到 id_rsa.pub 文件的内容


* 第 2 步：登陆 GitHub
>打开"Account settings"，"SSH Keys"页面：然后，点"Add SSH Key"，填上任意 Title，在 Key 文本框里粘贴 id_rsa.pub 文件的内容

### 添加远程库

* 要关联一个远程库，使用命令
```
git remote add origin git@server_name:path/repo_name.git
```
这里使用的命令是
`git remote add origin git@github.com:HuChanghong/learngit.git`


* 关联后，使用命令 `git push -u origin master` 第一次推送 master 分支的所有内容


* 此后，每次本地提交后，只要有必要，就可以使用命令 `git push origin master` 推送最新修改

>ssh 不行是因为你没有设置 ssh 秘钥  
>1：生成秘钥：`ssh-keygen -t rsa -C "你自己的邮箱" `  
>(这里不要设置密码，直接按回车就可以，以后更新就不需要密码)  
>2：` id_rsa` 这个文件是你的私钥、`id_rsa.pub` 是你的公共密钥，用记事本打开文件 `id_rsa.pub`, 把里面的内容复制到 github 配置 ssh  
>3：添加私秘钥到 ssh: `ssh-add id_rsa`（如果添加失败可以先执行命令 `ssh-agent bash`, 然后再次添加私秘钥。）  
>4: 用 `ssh -T git@github.com` 判断是否绑定成功。如果返回 successfully 表示成功  
### 从远程库克隆

* 要克隆一个仓库，首先必须知道仓库的地址，然后使用 `git clone` 命令克隆。
```
git clone git@github.com:HuChanghong/MarkdownStudy.git
```
* Git 支持多种协议，包括 `https`，但通过 `ssh` 支持的原生 `git` 协议速度最快。
### 取消链接并删除本地库
* 查看本地库关联了那些远程库 `git remote -v`
* 取消本地目录下关联的远程库 `git remote rm origin`
    * 此处 `origin` 是指你创建与远程库的链接的时候所使用的名字
* 删除本地库
    * `git branch` #显示本地所有分支
    * `git init` 1#初始化仓库 
    * `ls -a` 2#查看内部文件
    * `rm -rf .git` 3#强删.git
    * `cd ..` 5#回退
    * `rm -rf file_name` 6# 强删文件夹

## 分支管理

### 创建与合并分支
* 查看分支：`git branch`
* 创建分支：`git branch <name>`
* 切换分支：`git checkout <name>`
* 创建+切换分支：`git checkout -b <name>`
* 合并某分支到当前分支：`git merge <name>`
* 删除分支：`git branch -d <name>`

### 解决冲突

* 当 Git 无法自动合并分支时，就必须首先解决冲突。
* 解决冲突后，再提交，合并完成。
* 解决冲突就是把 Git 合并失败的文件手动编辑为我们希望的内容，再提交。
* 用 git log --graph 命令可以看到分支合并图。

>`git merge branch_name` 提示 `conflict`  
>`git status` 查看冲突的文件  
>打开这个文件, 修改保存  
>用带参数的 git log 也可以看到分支的合并情况：  
>`【git log --graph --pretty=oneline --abbrev-commit】`  
>最后删除不需要的分支  

### 分支管理策略

>在实际开发中，我们应该按照几个基本原则进行分支管理：首先，`master` 分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；那在哪干活呢？干活都在 `dev` 分支上，也就是说，`dev` 分支是不稳定的，到某个时候，比如 1.0 版本发布时，再把 `dev` 分支合并到 `master` 上，在 `master` 分支发布 1.0 版本；你和你的小伙伴们每个人都在 `dev` 分支上干活，每个人都有自己的分支，时不时地往 `dev` 分支上合并就可以了。


合并分支时，加上 `--no-ff` 参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而 `fast forward` 合并就看不出来曾经做过合并。

注意 `--no-ff` 参数，表示禁用 `Fast forward`
因为本次合并要创建一个新的 commit，所以加上 `-m` 参数，把 commit 描述写进去
```
【git merge --no-ff -m "balabala" branch_name】
```

### Bug 分支
>修复 bug 时，我们会通过创建新的 bug 分支进行修复，然后合并，最后删除；
>当手头工作没有完成时，先把工作现场 `git stash` 一下，然后去修复 bug，修复后，再 `git stash pop`，回到工作现场。


Git 提供了一个 `stash` 功能，可以把当前工作现场"储藏"起来，等以后恢复现场后继续工作
```
git stash
```
我理解的 `stash`, 就像是把当前的 `WIP` 压栈(这里 `WIP` 的含义就是所有修改了但未 commit 的数据)然后再一个一个取出来, 默认的 `git stash pop` 是按照自栈顶开始的顺序依次出栈, 也可以使用 `git stash pop stash@{num}` 的方式取出指定的 `WIP`
我们可以用 ` git stash list` 查看当前压栈了多少个 `WIP`

**注意:** `git stash` 不能将未被追踪的文件(untracked file)压栈, 也就是从未被 `git add` 过的文件, 也就是你在使用 `git status` 命令看到的提示 `Untracked files` 所列出的文件, 所以在 `git stash` 之前一定要用 `git status` 确认没有 `Untracked files`
**另外** 在有 `WIP` 没有 `commit` 或者 `stash` 的情况下是无法切换分支的

### Feature 分支

* 开发一个新 feature，最好新建一个分支；
* 如果要丢弃一个没有被合并过的分支，可以通过 `git branch -D <name>` 强行删除



### 多人协作
多人协作的工作模式通常是这样：

>* 首先，可以试图用 `git push origin <branch-name>` 推送自己的修改；
>* 如果推送失败，则因为远程分支比你的本地更新，需要先用 `git pull` 试图合并；
>* 如果合并有冲突，则解决冲突，并在本地提交；
>* 没有冲突或者解决掉冲突后，再用 `git push origin <branch-name>` 推送就能成功！
>* 如果 `git pull` 提示 `no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令 `git branch --set-upstream-to=origin/<branch-name> <branch-name>`。
>* 这就是多人协作的工作模式，一旦熟悉了，就非常简单。


* 查看远程库信息，使用 `git remote -v`；
* 本地新建的分支如果不推送到远程，对其他人就是不可见的；
* 从本地推送分支，使用 `git push origin branch-name`，如果推送失败，先用 `git pull` 抓取远程的新提交；
* 在本地创建和远程分支对应的分支，使用 `git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
* 建立本地分支和远程分支的关联，使用 `git branch --set-upstream-to=origin/branch-name branch-name`；
* 从远程抓取分支，使用 `git pull`，如果有冲突，要先处理冲突。

### rebase

[Git 中文手册-rebase](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%8F%98%E5%9F%BA)

<img src="https://git-scm.com/book/en/v2/images/basic-rebase-1.png" alt="分叉的提交历史。" style="zoom:50%;" />

Figure 36. 通过合并操作来整合分叉的历史

其实，还有一种方法：你可以提取在 `C4` 中引入的补丁和修改，然后在 `C3` 的基础上应用一次。 在 Git 中，这种操作就叫做 **变基（rebase）**。 你可以使用 `rebase` 命令将提交到某一分支上的所有修改都移至另一分支上，就好像"重新播放"一样。

在这个例子中，你可以检出 `experiment` 分支，然后将它变基到 `master` 分支上：

```console
$ git checkout experiment
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: added staged command
```

它的原理是首先找到这两个分支（即当前分支 `experiment`、变基操作的目标基底分支 `master`） 的最近共同祖先 `C2`，然后对比当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件， 然后将当前分支指向目标基底 `C3`, 最后以此将之前另存为临时文件的修改依序应用。 （译注：写明了 commit id，以便理解，下同）

<img src="https://git-scm.com/book/en/v2/images/basic-rebase-3.png" alt="将 `C4` 中的修改变基到 `C3` 上。" style="zoom:50%;" />

Figure 37. 将 `C4` 中的修改变基到 `C3` 上

现在回到 `master` 分支，进行一次快进合并。

```console
$ git checkout master
$ git merge experiment
```

<img src="https://git-scm.com/book/en/v2/images/basic-rebase-4.png" alt="`master` 分支的快进合并。" style="zoom:50%;" />

## 标签管理

>发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。Git 的标签虽然是版本库的快照，但其实它就是指向某个 commit 的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

*类似 IP 和域名的关系*

### 创建标签

* 命令 `git tag <tagname>` 用于新建一个标签，默认为 `HEAD`，也可以指定一个 `commit id`；
    * `git tag tag_name commit_id`
* 命令 `git tag -a <tagname> -m "blablabla..."` 可以指定标签信息；
    * 用 `-a` 指定标签名，`-m` 指定说明文字
* 命令 `git tag` 可以查看所有标签。
    * `git show tag_name` 查看标签信息

### 操作标签

命令 `git push origin <tagname>` 可以推送一个本地标签；
命令 `git push origin --tags` 可以推送全部未推送过的本地标签；
命令 `git tag -d <tagname>` 可以删除一个本地标签；
命令 `git push origin :refs/tags/<tagname>` 可以删除一个远程标签。

## 关联多个远程库
>使用多个远程库时，我们要注意，`git` 给远程库起的默认名称是 `origin`，如果有多个远程库，我们需要用不同的名称来标识不同的远程库。


先用 `git remote -v` 查看远程库信息

如果本地库已经关联了 `origin` 的远程库，并且，该远程库指向 `GitHub`。我们可以删除已有的 `GitHub` 远程库
```
git remote rm origin
```
使用如下命令关联远程库
```
git remote add remote_name git@server_name:path/repo_name.git
```

例如要关联 `Github` 远程库
```
git remote add github git@github.com:Github_id/repo_name.git
```
再比如要关联 `码云` 远程库
```
git remote add gitee git@gitee.com:Github_id/repo_name.git
```
此时要推送到不同的远程库则代码中使用对应的远程库名称(`remote_name`)
```
git push remote_name master
```
## 自定义 Git
### 忽略特殊文件

>有些时候，你必须把某些文件放到 Git 工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件啦，等等，每次 `git status` 都会显示 `Untracked files` ...，有强迫症的童鞋心里肯定不爽。好在 Git 考虑到了大家的感受，这个问题解决起来也很简单，在 `Git工作区的根目录下` 创建一个特殊的 `.gitignore` 文件，然后把要忽略的文件名填进去，Git 就会自动忽略这些文件。不需要从头写 `.gitignore` 文件，GitHub 已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：<https://github.com/github/gitignore>


**忽略文件的原则是：**

1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如 Java 编译产生的.class 文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

可以用 `git add -f` 强制添加被 `gitignore` 的文件
可以用 `git check-ignore` 命令检查是什么规则导致的无法添加文件
```
git check-ignore -v file_name
```

**注意:** `.gitignore` 文件本身要放到版本库里，并且可以对 `.gitignore` 做版本管理！



#### `Windows` 下创建 `.gitignore` 文件的常用方法
1. 在 `git bash` 中 `cd` 切换到根目录 `touch .gitignore` 命令创建
2. 在资源管理器创建文件时，文件命名"`.gitignore.`"，注意结尾有个.号，回车确认时系统会自动存成 `.gitignore`
3. 打开文本编辑器，保存时文件名输入"`.gitignore`"，保存类型选"`所有文件`"
4. 进入 cmd 命令行，执行 `echo > .gitignore` 输入空内容并创建文件，或执行 `rename somefile .gitignore`、`copy somefile .gitignore` 从已有文件复制、重命名。

### 配置别名
```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
配置 Git 的时候，加上 `--global` 是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。
每个仓库的 Git 配置文件都放在 `.git/config` 文件, 打开配置文件, 别名就在 `[alias]` 后面，要删除别名，直接把对应的行删掉即可
当前用户的 Git 配置文件放在 `用户主目录` 下的一个隐藏文件 `.gitconfig` 中

查看用户的配置信息使用 `git config --global --list` 命令
查看当前仓库的配置信息使用 `git config --local --list` 命令
#### 删除别名
- 命令删除已定义的别名 `git config --global --unset alias.ci `
- 直接删除已定义的别名 `cd .git` `vi config` 打开对应的配置文件, 别名就在 `[alias]` 后面，直接把对应的行删掉即可

gitconfig 配置文件的读取顺序：

- linux 中类似用户的配置文件一样有 3 层，系统级，用户级，项目级。 
- windows 也基本一样，但常常只存在于是用户根目录(C:\User\xxx)，项目目录中。 最下层的覆盖上面的，alias 配置也在其中，手动改配置文件也和命令一样。 加了--global 选项的，表示配置写到了用户级，--system 是系统级，win 是在安装目录（如 C:\Program Files\Git\mingw64\etc），不加就在项目级

# SVN 笔记

## Subversion

[Apache Subversion: Quick Start](https://subversion.apache.org/quick-start)

`checkout` 相当于 git 的 `clone`

`update` 相当于 git 的 `pull`

`commit` 相当于 git 的 `push`