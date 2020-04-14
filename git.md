# git

## git 简介

- 分布式版本控制系统

### 注册 git

```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

> 注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

---

### 初始化添加到 git

**repository**

- 版本库
- 类似于一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

---

**第一步：**创建仓库，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录

```bash
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```

>`pwd`命令用于显示当前目录。

**第二步：**通过`git init`命令把这个目录变成Git可以管理的仓库：

```bash
$ git init
```

瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。

**第三步：**把文件添加到版本库

> 把要上传到仓库的文件放到刚刚创建的目录，也可以放到刚刚文件的子目录

- 用命令`git add`告诉Git，把文件添加到仓库：

```bash
$ git add test.txt 
```

```bash
# . 表示保存所有的文件
$ git add .
```

- 用命令`git commit`告诉Git，把文件提交到仓库：

```bash
$ git commit -m "Hello git"
```

> 简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

> `git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`2 insertions`：插入了两行内容（readme.txt有两行内容）

- 为什么Git添加文件需要`add`，`commit`一共两步呢？因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：

```bash
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

**小结**

现在总结一下今天学的两点内容：

初始化一个Git仓库，使用`git init`命令。

添加文件到Git仓库，分两步：

1. 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
2. 使用命令`git commit -m <message>`，完成。

> git add是保存到暂存区，commit才是提交到服务器上

```bash
# 清空 git 仓库
$ rm -rf  .git
```

---

### 修改文件添加到 git

- 修改文件后运行`git status`命令看看结果：会发现修改的文件呈红色表示修改了但是未提交

```bash
$ git status
```

- 使用命令`git diff` 查看更改的内容

```bash
$ git diff test.txt 
```

- 再次使用 `git add` 指令提交修改的文件：

```bash
$ git add test.txt 
```

- `git status` 查看状态：文件呈绿色表示文件已提交到暂存区，并告诉我们，将要被提交的修改包括`readme.txt`，下一步，就可以放心地提交了：

```bash
$ git commit -m "add hello git"
```

- 提交后，我们再用`git status`命令看看仓库的当前状态：Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working tree clean）的。

```bash
$ git status
On branch master
nothing to commit, working tree clean
```

**小结**

- 要随时掌握工作区的状态，使用`git status`命令。
- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

----

## 时间机穿梭机

### 版本回退

- 像这样，你不断对文件进行修改，然后不断提交修改到版本库里，就好比玩RPG游戏时，每通过一关就会自动把游戏状态存盘，如果某一关没过去，你还可以选择读取前一关的状态。有些时候，在打Boss之前，你会手动存盘，以便万一打Boss失败了，可以从最近的地方重新开始。Git也是一样，每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为`commit`。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个`commit`恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

- 使用 `git log` 查看文件提交的记录

```bash
$ git log
```

- 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

```bash
$ git log --pretty=oneline
```

- 首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。
- 现在，我们要把当前版本`append GPL`回退到上一个版本`add distributed`，就可以使用`git reset`命令：

```bash
$ git reset --hard HEAD^
```

```bash
# cat test.txt 查看文件内容
$ cat test.txt
hello git
```

```bash
# 回到以前的版本，这时工作区的内容也会退到以前的版本
$ git reset -- hard 'commit id'
```

> reset：调整

> 版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了

- 在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`append GPL`，就必须找到`append GPL`的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令：

```bash
$ git reflog
```

**小结**

现在总结一下：

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

> log：日志

---

### 工作区和暂存区

**工作区（Working Directory）**

- 就是你在电脑里能看到的目录

**版本库（Repository）**

- 工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

- Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

- 前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

  - 第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

  - 第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

- 因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

- 俗话说，实践出真知。现在，我们再练习一遍，先对`test.txt`做个修改，然后，在工作区新增一个`LICENSE`文本文件（内容随便写）。

  - 再使用 `git status` 查看状态
  - Git非常清楚地告诉我们，`test.txt`被修改了，而`LICENSE`还从来没有被添加过，所以它的状态是`Untracked`。

- 所以，`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。

- 一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的

**小结**

- 暂存区是Git非常重要的概念，弄明白了暂存区，就弄明白了Git的很多操作到底干了什么。

---

### 管理修改

- 第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`

**小结**

现在，你又理解了Git是如何跟踪修改的，每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。

---

### 撤销修改

- `git checkout -- file`可以丢弃工作区的修改

- 命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

  1. `readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

  2. `readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

  + 总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

**小结**

又到了小结时间。

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节，不过前提是没有推送到远程库。

---

### 删除文件

- 一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了：

```bash
$ rm test.txt
```

- `git status`命令会立刻告诉你哪些文件被删除了：

```bash
$ git status
```

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

```bash
$ git rm test.txt
rm 'test.txt'

# 需要重新 commit ,记录删除了那些文件
$ git commit -m "remove test.txt"
```

> 小提示：先手动删除文件，然后使用git rm <file>和git add<file>效果是一样的。

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```bash
$ git checkout -- test.txt
```

- `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

>  注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！

**小结**

- 命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。

- 如果你用的rm删除文件，那就相当于只删除了工作区的文件，如果想要恢复，直接用git checkout -- <file>就可以 
- 如果你用的是git rm删除文件，那就相当于不仅删除了文件，而且还添加到了暂存区，需要先git reset HEAD <file>，然后再git checkout -- <file> 
- 如果你想彻底把版本库的删除掉，先git rm，再git commit 就ok了

> 会失去修改了但是没有 git commit 的内容

> 记得注意空格啊啊啊啊啊，不要加多空格或者少写空格

> hub：中心，checkout：付款台

---

## 远程仓库

- 创建 SSH key `$ ssh-keygen -t rsa -C "youremail@example.com"`
- 用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

- 为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

  当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

  最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。

  如果你不想让别人看到Git库，有两个办法，一个是交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。另一个办法是自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。这个方法我们后面会讲到的，相当简单，公司内部开发必备。

**小结**

- “有了远程仓库，妈妈再也不用担心我的硬盘了。”——Git点读机

---

### 添加远程库

- 需要在 github 上创建仓库
- 先关联后 `git add` `git commit`...
- 按照提示指令关联远程仓库：

```bash
$ git remote add origin https://github.com/609529897/git.git
```

> remote 遥远的   origin 源头

- 下一步，就可以把本地库的所有内容推送到远程库上：

```bash
$ git push -u origin master
```

- 把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

- 由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

- 从现在起，只要本地作了提交，就可以通过命令：

```bash
$ git push origin master
```

- 把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

**SSH警告**

- 当你第一次使用Git的`clone`或者`push`命令连接GitHub时，会得到一个警告：

```
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```

- 这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入`yes`回车即可。

- Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：

```
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
```

- 这个警告只会出现一次，后面的操作就不会有任何警告了。

- 如果你实在担心有人冒充GitHub服务器，输入`yes`前可以对照[GitHub的RSA Key的指纹信息](https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/)是否与SSH连接给出的一致。

---

**小结**

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

---

### 从远程库克隆

- 上次我们讲了先有本地库，后有远程库的时候，如何关联远程库。

  现在，假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。

  首先，登陆GitHub，创建一个新的仓库，名字叫`gitskills`：

- 我们勾选`Initialize this repository with a README`，这样GitHub会自动为我们创建一个`README.md`文件。创建完毕后，可以看到`README.md`文件。

- 克隆远端仓库到本地

```bash
$ git clone git@github.com:609529897/new-app.git
```

> ls 命令行指令，查看文件夹内目录

- 如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

- 你也许还注意到，GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址。实际上，Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。

- 使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`。

**小结**

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快。

---

## 分支管理

### 创建并合并分支

- 创建一个 `dev` 分支，让后切换到 `dev` 分支：

```bash
$ git checkout -b dev
```

- `git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```bash
$ git branch dev
$ git checkout dev
```

- 然后，用`git branch`命令查看当前分支：绿色标`*`表示现在在哪个分支

```bash
$ git branch
```

> branch 分支

- 然后，我们就可以在`dev`分支上正常提交

```bash
$ git add test.txt
$ git commit -m "branch test"
```

- 切换到 `master` 分支

```bash
$ git checkout master
```

> commit 做出

- 我们把`dev`分支的工作成果合并到`master`分支上：

```bash
$ git merge dev
```

> merge 合并

- 删除分支

```bash
$ git branch -d dev
```

- 因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

**switch**

>  switch 命令不好使，可能是因为我电脑的 git 版本过低

- 实际上，切换分支这个动作，用`switch`更科学。因此，最新版本的Git提供了新的`git switch`命令来切换分支：

- 创建并切换到新的`dev`分支，可以使用：

```bash
$ git switch -c dev
```

- 直接切换到已有的`master`分支，可以使用：

```bash
$ git switch master
```

**小结**

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`或者`git switch <name>`

创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

---

### 解决冲突

- 当主分支 `master` 和别的分支在同一进度，但是 `git commit` 的内容不一样，但是想要合并这时会产生冲突

- 当合并 `git merge featurel`  时会提示产生冲突，`git status` 也可以告诉我们冲突的文件：

```bash
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

- 在文件里也会显示

```txt
<<<<<<< HEAD
remove
=======
add
>>>>>>> featurel
```

- Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改后保存并重新 `git add`，`git commit` 即可。

- 在删除 `featurel` 即可。
- 用带参数的`git log`也可以看到分支的合并情况：

```bash
$ git log --graph --pretty=oneline --abbrev-commit
*   cf810e4 (HEAD -> master) conflict fixed
|\  
| * 14096d0 (feature1) AND simple
* | 5dc6824 & simple
|/  
* b17d20e branch test
* d46f35e (origin/master) remove test.txt
* b84166e add test.txt
* 519219b git tracks changes
* e43a48b understand how stage works
* 1094adb append GPL
* e475afc add distributed
* eaadf4e wrote a readme file
```

**小结**

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。

---

### 分支管理策略

- 通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。
- 如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

- 下面我们实战一下`--no-ff`方式的`git merge`：
- 首先，仍然创建并切换`dev`分支：

```bash
$ git checkout -b dev
```

- 修改readme.txt文件，并提交一个新的commit：

```bash
$ git add readme.txt 
$ git commit -m "add merge"
```

- 现在，我们切换回`master`：

```bash
$ git checkout master
```

- 准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：

````bash
$ git merge --no-ff -m "merge with no-ff" dev
````

- 因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。

- 不使用`Fast forward`模式，`master` 会领先 `dev` 分支一步。

**分支策略**

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

![git-br-policy](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)

**小结**

Git分支十分强大，在团队开发中应该充分应用。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

---

### bug 分支

- 软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

- 当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在`dev`上进行的工作还没有提交

- 并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

- 幸好，Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```bash
$ git stash
```

- 现在，用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

- 首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

```bash
$ git checkout master
$ git checkout -b issue-101
```

- 现在修复bug，然后提交：

```bash
$ git add readme.txt 
$ git commit -m "fix bug 101"
```

- 修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：

```bash
$ git checkout master
$ git merge --no-ff -m "merged bug fix 101" issue-101
```

- 太棒了，原计划两个小时的bug修复只花了5分钟！现在，是时候接着回到`dev`分支干活了！

```bash
$ git checkout dev
$ git status
```

- 工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

```bash
$ git stash list
```

- 工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

- 一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

- 另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：

```bash
$ git stash pop
```

- 再用`git stash list`查看，就看不到任何stash内容了：

```bash
$ git stash list
```

- 你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```bash
$ git stash apply stash@{0}
```

- 同样的bug，要在dev上修复，我们只需要把`4c805e2 fix bug 101`这个提交所做的修改“复制”到dev分支。注意：我们只想复制`4c805e2 fix bug 101`这个提交所做的修改，并不是把整个master分支merge过来。

- 为了方便操作，Git专门提供了一个`cherry-pick`命令，让我们能复制一个特定的提交到当前分支：

```bash
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
[master 1d4b803] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

- Git自动给dev分支做了一次提交，注意这次提交的commit是`1d4b803`，它并不同于master的`4c805e2`，因为这两个commit只是改动相同，但确实是两个不同的commit。用`git cherry-pick`，我们就不需要在dev分支上手动再把修bug的过程重复一遍。

- 有些聪明的童鞋会想了，既然可以在master分支上修复bug后，在dev分支上可以“重放”这个修复过程，那么直接在dev分支上修复bug，然后在master分支上“重放”行不行？当然可以，不过你仍然需要`git stash`命令保存现场，才能从dev分支切换到master分支。

**小结**

- 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

- 当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；

- 在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

---

### Feature分支

- 当想要开发一个新的功能，建议除了 `master` 分支和工作分支 `dev` 外新建一个分支。

- 当想要 `git branch -d feature-vulcan` 删除一个未合并的分支时，会提示没有合并，只能使用

  `git branch -D <name>` 强制删除分支。

**小结**

- 开发一个新feature，最好新建一个分支；

- 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

---

### 多人协作

- 当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

- 要查看远程库的信息，用`git remote`：

- 或者，用`git remote -v`显示更详细的信息：

  ```bash
  $ git remote
  origin
  ```

### 推送分支

- 推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```bash
$ git push origin master
```

- 如果要推送其他分支，比如`dev`，就改成：

```bash
$ git push origin dev
```

- 但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？
  - `master`分支是主分支，因此要时刻与远程同步；
  - `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
  - bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
  - feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

---

### 抓取分支

- 多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。

- 现在，模拟一个你的小伙伴，可以在另一台电脑（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆：

```bash
$ git clone git@github.com:michaelliao/learngit.git
```

- 当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的`master`分支。

- 现在，你的小伙伴要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，于是他用这个命令创建本地`dev`分支：

```bash
$ git checkout -b dev origin/dev
```

- 现在，他就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程：

```bash
$ git add env.txt
$ git commit -m "add env"
$ git push origin dev
```

- 你的小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送

- 推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

  ```bash
  $ git pull
  ```

- `git pull`也失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：

  ```bash
  $ git branch --set-upstream-to=origin/dev dev
  ```

- 再pull：

  ```bash
  $ git pull
  ```

- 这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的[解决冲突](http://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)完全一样。解决后，提交，再push：

```bash
$ git commit -m "fix env conflict"
$ git push origin dev
```

**因此，多人协作的工作模式通常是这样：**

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

### 小结

- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

---

### Rebase

- `git rebase`操作的特点：把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。

**小结**

- rebase操作可以把本地未push的分叉提交历史整理成直线；
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

---

## 标签管理

- 发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

- Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

### 创建标签

- 在Git中打标签非常简单，首先，切换到需要打标签的分支上：

```bash
$ git branch
$ git checkout master
```

然后，敲命令`git tag <name>`就可以打一个新标签：

```bash
$ git tag v1.0
```

可以用命令`git tag`查看所有标签：

```bash
$ git tag
```

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：

```bash
$ git log --pretty=oneline --abbrev-commit
```

比方说要对`add merge`这次提交打标签，它对应的commit id是`f52c633`，敲入命令：

```bash
$ git tag v0.9 f52c633
```

再用命令`git tag`查看标签：

```bash
$ git tag
v0.9
v1.0
```

注意，标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息：

```bash
$ git show v0.9
```

可以看到，`v0.9`确实打在`add merge`这次提交上。

还可以创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：

```bash
$ git tag -a v0.1 -m "version 0.1 released" 1094adb
```

用命令`git show <tagname>`可以看到说明文字：

```bash
$ git show v0.1
```

> 注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

**小结**

- 命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；
- 命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
- 命令`git tag`可以查看所有标签。

---

### 操作标签

- 如果标签打错了，也可以删除：

```bash
$ git tag -d v0.1
```

- 因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

- 如果要推送某个标签到远程，使用命令`git push origin <tagname>`：

```bash
$ git push origin v1.0
```

或者，一次性推送全部尚未推送到远程的本地标签：

```bash
$ git push origin --tags
```

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

```bash
$ git tag -d v0.9
```

然后，从远程删除。删除命令也是push，但是格式如下：

```bash
$ git push origin :refs/tags/v0.9
```

**小结**

- 命令`git push origin <tagname>`可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

---

## 使用GitHub

- 从 Github 克隆

```bash
git clone git@github.com:michaelliao/bootstrap.git
```

- 一定要从自己的账号下clone仓库，这样你才能推送修改。如果从bootstrap的作者的仓库地址`git@github.com:twbs/bootstrap.git`克隆，因为没有权限，你将不能推送修改。

- 如果你想修复bootstrap的一个bug，或者新增一个功能，立刻就可以开始干活，干完后，往自己的仓库推送
- 如果你希望bootstrap的官方库能接受你的修改，你就可以在GitHub上发起一个pull request。当然，对方是否接受你的pull request就不一定了。

**小结**

- 在GitHub上，可以任意Fork开源仓库；
- 自己拥有Fork后的仓库的读写权限；
- 可以推送pull request给官方仓库来贡献代码。

---

## 自定义 git

- 让Git显示颜色，会让命令输出看起来更醒目：

```bash
$ git config --global color.ui true
```

### 忽略特殊文件

- 有些时候，你必须把某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件啦，等等，每次`git status`都会显示`Untracked files ...`，有强迫症的童鞋心里肯定不爽。

- 好在Git考虑到了大家的感受，这个问题解决起来也很简单，在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

- 不需要从头写`.gitignore`文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore

**忽略文件的原则是：**

1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class`文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

- 后一步就是把`.gitignore`也提交到Git，就完成了！当然检验`.gitignore`的标准是`git status`命令是不是说`working directory clean`。

- 使用Windows的童鞋注意了，如果你在资源管理器里新建一个`.gitignore`文件，它会非常弱智地提示你必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为`.gitignore`了。

- 如果你确实想添加该文件，可以用`-f`强制添加到Git：

```bash
$ git add -f App.class
```

- 或者你发现，可能是`.gitignore`写得有问题，需要找出来到底哪个规则写错了，可以用`git check-ignore`命令检查：

```bash
$ git check-ignore -v App.class
.gitignore:3:*.class	App.class
```

- Git会告诉我们，`.gitignore`的第3行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。

**小结**

- 忽略某些文件时，需要编写`.gitignore`；
- `.gitignore`文件本身要放到版本库里，并且可以对`.gitignore`做版本管理！

---

### 配置别名

- 我们只需要敲一行命令，告诉Git，以后`st`就表示`status`：

```bash
$ git config --global alias.st status
```

- 当然还有别的命令可以简写，很多人都用`co`表示`checkout`，`ci`表示`commit`，`br`表示`branch`：

```bash
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
```

- `--global`参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。

- 在[撤销修改](https://www.liaoxuefeng.com/wiki/896043488029600/897889638509536)一节中，我们知道，命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个unstage操作，就可以配置一个`unstage`别名：

```bash
$ git config --global alias.unstage 'reset HEAD'
```

- 当你敲入命令：

```bash
$ git unstage test.py
```

- 实际上Git执行的是：

```bash
$ git reset HEAD test.py
```

配置一个`git last`，让其显示最后一次提交信息：

```bash
$ git config --global alias.last 'log -1'
```

这样，用`git last`就能显示最近一次的提交：

```bash
$ git last
commit adca45d317e6d8a4b23f9811c3d7b7f0f180bfe2
Merge: bd6ae48 291bea8
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Thu Aug 22 22:49:22 2013 +0800

    merge & fix hello.py
```

甚至还有人丧心病狂地把`lg`配置成了：

```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

来看看`git lg`的效果：

![git-lg](https://www.liaoxuefeng.com/files/attachments/919059728302912/0)

**配置文件**

- 配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

- 配置文件放哪了？每个仓库的Git配置文件都放在`.git/config`文件中：

```bash
$ cat .git/config 
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
    ignorecase = true
    precomposeunicode = true
[remote "origin"]
    url = git@github.com:michaelliao/learngit.git
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
    remote = origin
    merge = refs/heads/master
[alias]
    last = log -1
```

- 别名就在`[alias]`后面，要删除别名，直接把对应的行删掉即可。

- 而当前用户的Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中：

```bash
$ cat .gitconfig
[alias]b
    co = checkout
    ci = commit
    br = branch
    st = status
[user]
    name = Your Name
    email = your@email.com
```

- 配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置。

**小结**

- 给Git配置好别名，就可以输入命令时偷个懒。我们鼓励偷懒。

---

### 搭建Git服务器

- 搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian，这样，通过几条简单的`apt`命令就可以完成安装。

假设你已经有`sudo`权限的用户账号，下面，正式开始安装。

第一步，安装`git`：

```bash
$ sudo apt-get install git
```

第二步，创建一个`git`用户，用来运行`git`服务：

```bash
$ sudo adduser git
```

第三步，创建证书登录：

收集所有需要登录的用户的公钥，就是他们自己的`id_rsa.pub`文件，把所有公钥导入到`/home/git/.ssh/authorized_keys`文件里，一行一个。

第四步，初始化Git仓库：

先选定一个目录作为Git仓库，假定是`/srv/sample.git`，在`/srv`目录下输入命令：

```bash
$ sudo git init --bare sample.git
```

Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以`.git`结尾。然后，把owner改为`git`：

```bash
$ sudo chown -R git:git sample.git
```

第五步，禁用shell登录：

出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑`/etc/passwd`文件完成。找到类似下面的一行：

```bash
git:x:1001:1001:,,,:/home/git:/bin/bash
```

改为：

```bash
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```

这样，`git`用户可以正常通过ssh使用git，但无法登录shell，因为我们为`git`用户指定的`git-shell`每次一登录就自动退出。

第六步，克隆远程仓库：

现在，可以通过`git clone`命令克隆远程仓库了，在各自的电脑上运行：

```bash
$ git clone git@server:/srv/sample.git
Cloning into 'sample'...
warning: You appear to have cloned an empty repository.
```

剩下的推送就简单了。

**管理公钥**

如果团队很小，把每个人的公钥收集起来放到服务器的`/home/git/.ssh/authorized_keys`文件里就是可行的。如果团队有几百号人，就没法这么玩了，这时，可以用[Gitosis](https://github.com/res0nat0r/gitosis)来管理公钥。

这里我们不介绍怎么玩[Gitosis](https://github.com/res0nat0r/gitosis)了，几百号人的团队基本都在500强了，相信找个高水平的Linux管理员问题不大。

**管理权限**

有很多不但视源代码如生命，而且视员工为窃贼的公司，会在版本控制系统里设置一套完善的权限控制，每个人是否有读写权限会精确到每个分支甚至每个目录下。因为Git是为Linux源代码托管而开发的，所以Git也继承了开源社区的精神，不支持权限控制。不过，因为Git支持钩子（hook），所以，可以在服务器端编写一系列脚本来控制提交等操作，达到权限控制的目的。[Gitolite](https://github.com/sitaramc/gitolite)就是这个工具。

这里我们也不介绍[Gitolite](https://github.com/sitaramc/gitolite)了，不要把有限的生命浪费到权限斗争中。

**小结**

- 搭建Git服务器非常简单，通常10分钟即可完成；
- 要方便管理公钥，用[Gitosis](https://github.com/sitaramc/gitolite)；
- 要像SVN那样变态地控制权限，用[Gitolite](https://github.com/sitaramc/gitolite)。

---

> [Git Cheat Sheet](https://gitee.com/liaoxuefeng/learn-java/raw/master/teach/git-cheatsheet.pdf)
>
> 官方网站：[http://git-scm.com](http://git-scm.com/)

> 每个仓库都可以有多个分支，一般 master 是主分支发布产品用的程序员很少动它，dev 是大家工作的分支，而你需要单独自己创建一个分支。最后跟 dev 分支 merge 并重新 push 到 dev。

**修改代码提交**

- git add .

- git commit -m "hello git"

- git push
---

## VSCode 使用 git

- 使用 vscode 打开你正在使用 git 管理的仓库。

- 点击加号等于 `git add`

- 点击对号提交，等于`git commit`

- 最后点击底部的提交，等于`git push`