## 是什么

最先进的分布式版本控制系统

## 用处

1. 版本控制和管理

   可以查看某一个版本，回退到任意一个版本

2. 协作编辑（开发）：

   可以查看别人的改动，并且和自己现在的改动进行对比，合并



## 历史

要从linus和linux说起

linus为了管理linux，一开始是用手工管理，尽管当时有cvs和svn这样的**集中式版本版本控制系统**，但是他们速度慢，而且要联网，他不喜欢。有一些商用的好用一点的，但是要付费，他也不喜欢

linus为了管理linux，使用一家公司的版本控制系统（公司给linux社区使用），但是后面有了矛盾，于是linux用了**2周**时间自己用C语言写了一个**分布式的版本控制系统**



## 集中式 VS 分布式

### 集中式

集中式要有**中央服务器**进行版本控制和管理     **必须联网**，以前网速慢，让人奔溃  



### 分布式

分布式**每台电脑**都是都可以进行版本控制和管理

局域网内每台电脑把修改推送给对方，就可以看到对方的修改



但是由于不一定是同一个局域网，或者对方可能没有开机，所以一般还是会有**中央服务器**进行修改的交换，但是没有也可以在本地进行版本控制和管理



Git对比与SVN，他还有强大的**分支管理**



## 安装

需要配置

```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对**某个仓库**指定不同的用户名和Email地址



## 创建版本库

版本库就是仓库，可以理解为目录，这目录里面**所有的文件都可以被Git管理**，每个文件的改动都会被追踪和记录

### 创建

`git init`



所有的版本控制系统，只能追踪**文本文件**的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外

而图片，视频这些**二进制文件**，没办法追踪文件的变化

Microsoft 的word格式的文件也是二进制文件



因为文本是有编码的，比如中文常用的GBK编码，日文有Shift_JIS编码，建议统一使用UFT-8编码

不建议使用window自带的笔记本，推荐使用nodepad++



### 提交到本地仓库

`git add`

`git commit`



## 时光机穿梭

### 查看修改

`git status`   查看当前仓库的改动状态

`git diff xx文件`	查看difference，显示的格式是Unix通用的diff格式



### 版本回退

每当你觉得文件修改到一定程度的时候，就可以“**保存一个快照**”，这个**快照**在Git中被称为`commit`。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个`commit`恢复，然后继续工作，而不是把几个月的工作成果全部丢失



#### 查看**快照记录**

`git log`  显示最近到最远的提交日志

可以加参数`--pretty=oneline`，也就是`git log --pretty=oneline` 就可以一行显示一次的提交就



#### commit id

git的快照的commit id（版本号）和svn不一样，他用的是SHA1值，保证不会重复



#### 退回上一个版本

`git reset --hard HEAD^`   //上上个版本是`HEAD^^`    往上100个版本是`HEAD~100`

此时，使用`git log`提交，发现最新的版本就是你目前到的这个，再之前的已经不见了





#### 退回到某一个版本

需要知道cmmit id，比如上面git log显示的(窗口没有关掉，上面的命令还在)

`git reset --hard 1094a`

 版本号没必要写全，git会自动去找。当然不能写1，2位，因为可能找到不止一个



git版本回退是让**Head指针指向**对应的**版本号**，顺便把**工作区的文件更新**了，所以版本回退很快



#### 回到未来

`git reflog`查看**命令历史**，以便确定要回到未来的哪个版本



### 工作区和暂存区

工作区：

你能在电脑里面看到的目录

暂存区：

工作区里面有一个`.git`的隐藏目录，这是版本库（本地仓库）



版本库

如下图，版本库里面包含3个东西：

一个是暂存区（stage）

一个是**git为我们自动创建**的第一个分支**master**

一个是指向master的一个指针叫head



![0.jpg](https://i.loli.net/2020/02/07/cS4WXt1DPTFsfjr.jpg)



当我们执行`git add`的时候，就是把要提交的所有修改放到暂存区（stage）

当我们执行`git commit`的时候，就是把暂存区的所有修改放到分支



一旦提交之后，分支和工作区是一样的，这时候，工作区就是“干净的”





### 管理修改

Git**跟踪并管理**的是**修改**，而非文件

可以用`git diff HEAD -- readme.txt`  查看工作区和版本库里面最新版本的区别



### 撤销修改

场景一：

当你改了工作区的某个文件的内容，想直接**丢弃工作区的修改**时，用命令`git checkout -- 某个file`

场景二：

当你提交到了**暂存区想撤回**的时候，使用命令`git reset HEAD 某个file`

场景三：

当你已经提交到了**分支**，想进行**版本的回退**的时候，使用`git reset --hard commitId`



### 删除文件

你可以`rm 某个file`然后再加到暂存区 `git add 某个file`再`git commit`

也可以使用`git rm 某个file`然后再`git commit`



如果在工作区误删除了某一个文件，则使用上面提到的`git checkout -- 某个file`

`git checkout`其实是用**版本库里的版本**替换**工作区的版本**，无论工作区是修改还是删除，都可以恢复

注意：

如果之前没有commit过该文件，则无法恢复工作区里面的文件



## 远程仓库

这里使用Github的服务器：

由于你的本地Git仓库和GitHub仓库之间的**传输是通过SSH加密**的，需要设置：

1.创建SSH key

在用户目录下，看一下有没有`.ssh`目录，如果有，再看看这目录有没有`id_rsa`和`id_rsa.pub`



如果没有，则在git bash中输入`ssh-keygen -t rsa -C "your e-mailexample.com"`

然后一路回车，使用默认值，由于这key也不是用于军事目的，因此也无需设置密码。



顺利的话，用户目录就会出现上面讲的2个东西

其中`id_rsa`是私钥，`id_rsa.pub`是公钥



你需要将公钥添加到github中，github需要根据他去识别用户



### 添加远程库

在github创建了一个远程仓库之后，你可以克隆，也可以关联

克隆：`git clone git@github.com:Wenhao-liao/VUE-DEMO.git`

关联：`git remote add origin git@github.com:Wenhao-liao/VUE-DEMO.git`



关联之后，就可以把本地仓库的所有内容推送到远程库上：

`git push -u origin master`

由于远程仓库是空的，我们第一次推送master分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容**推送**的远程的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令



用`git clone git@github.com:Wenhao-liao/VUE-DEMO.git` 可以克隆项目到本地仓库

也可以使用https协议进行克隆，但是他速度慢，最大麻烦是每次都必须输入账号密码，有些公司只开放http端口的时候使用



## 分支管理

### 创建和合并和删除分支

每次**提交**，git都会把他们**穿成一条时间线**，这个时间线就是一条**分支**

一开始git会创建一个`master`分支，有一个`master`指针，`HEAD`指针指向他最新的提交

![0.png](https://i.loli.net/2020/02/11/YpG9mBIkTDKjnPU.png)





**新创建**一个`dev`分支去开发，**切换**到该分支( 同时`HEAD`会指向他 )，新提交一次，这分支往前移一步，而`master`分支不变

![l.png](https://i.loli.net/2020/02/11/ZcYUSxhseW1vPpo.png)



![l _1_.png](https://i.loli.net/2020/02/11/NB7vRUWpJAitEkQ.png)

然后再将用于开发的`dev`分支合并到主分支`master`上( 将`master`指针指向指向`dev`的当前提交 )

这个`dev`分支就是自己创建的一个开发分支，可以瞎几把改，最后选择合并或者不合并

![0 _1_.png](https://i.loli.net/2020/02/11/5pmkeWPxM4SKgDE.png)



分支合并后，可以删除原来用于临时开发的`dev`分支。

删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下一条`master`分支

![0 _2_.png](https://i.loli.net/2020/02/11/T1OcdQhguoIM67j.png)





下面是用命令行实际操作上面的内容

首先，创建`dev`分支，然后切换到`dev`分支（工作区不变，`dev`指针指向原来`master`的最新提交）：

```bash
$ git checkout -b dev
Switched to a new branch 'dev'
```



`git checkout`命令加上`-b`参数表示并切换，相当于以下2条命令：

```bash
$ git branch dev
$ git checkout dev
Switch to branch 'dev'
```



然后，用`git branch`命令查看当前分支，当前分支前面会标一个`*`号

```bash
$ git branch
* dev
  master
```



切完分支在该分支操作：修改工作区，提交到暂存区，提交到分支（现在是`dev`）



下面要把`dev`分支合并到`master`主分支

首先切换回主分支：

```bash
git checkout master
Switch to branch 'master'
```



现在，把`dev`的工作成果合并到`master`分支上：

```bash
git merge dev
```

将落后一个提交的`master`与`dev`合并，这个操作超级快，因为是`master`指针直接指向`dev`分支最新的提交



合并完之后删除`dev`分支

```bash
git branch -d dev
```



因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，**合并后再删掉分支**，这和直接在`master`分支上工作效果是一样的，但过程更安全。



切换分支使用`git checkout <branch> `

前面讲了撤销工作区修改用的是`git checkout -- [file] `

同一个命令有2个作用



所以最新版本的git更新了`switch`用于切换分支：

创建并切换新的分支`dev`，可以使用：

```bash
dev switch -c dev
```

直接切换到已有的`master`分支，可以使用：

```bash
git switch master
```



分支总结：

git鼓励大量使用分支

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`和`git switch <name>`

创建+切换分支：`git checkout -b <name>`和`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`



### 解决冲突

`dev`分支

`master`分支



`master`分支和`feature1`分支各自都**分别有新的提交**的时候，就会变成这样：

![0 _2.png](https://i.loli.net/2020/02/11/DgkGRsp8iVOwPdo.png)









这种情况下，git无法执行“**快速合并**”，只能试图把各自的修改**拼接**起来，但是这样的合并就可能会有冲突

```bash
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```



这时候要手动去解决冲突：

```txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容



手动解决完冲突之后再提交

```bash
$ git add readme.txt 
$ git commit -m "conflict fixed"
[master cf810e4] conflict fixed
```

然后就会变成下图这样：

![0 _1_.png](https://i.loli.net/2020/02/11/uaNS8trikvBTgem.png)







使用带参数的`git log`命令可以看到分支的合并情况：

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

上面的参数：

```txt
pretty=oneline：一行显示，只显示哈希值和提交说明
--graph：显示ASCII图形表示的分支合并历史
--abbrev-commit：仅显示SHA-1的前几个字符，而非所有的40个字符
```



最后删除`dev`分支

```bash
git branch -d dev
```



### 分支管理策略

#### 合并后能看出分支信息

通常，在合并时，如果可能，Git会用`Fast Forward`模式，但这种模式下，**删除分支后，会丢掉分支信息**

如果要强制禁用`Fast-forward`模式，Git就会在merge时生成一个新的commit，这样，从历史分支上就可以看出分支信息。

需要达到上述效果，我们需要使用 `--no-ff`方式的`git merge` :

```bash
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

由于在merge之后，其实是生成一个新的commit，所以需要填写-m信息

![0 _1_.png](https://i.loli.net/2020/02/11/BsDiZUdF5SOlNu7.png)



上面用默认的`Fast-forward`模式，则删除后分支就和没有存在过一样，但是在merge的时候用了`--no-ff`之后

``` bash
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
...
```

#### 分支管理策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如发布版本的时候，将`dev`合并到`master`，然后发布`master`

成员之间每个人都在`dev`上干活，每个人都有自己的分支，时不时往`dev`分支合并就可以了

过程如下图：



![0.png](https://i.loli.net/2020/02/11/y9f6pQYehSHMUbk.png)



### Bug分支

创建一个临时分支来修复Bug，修复后合并，然后再将临时分支删除

但是有这样的情景：

你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug

这时候可以使用`git stash`功能，可以把当前工作现场存储起来，等以后恢复后继续工作

```bash
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```



使用了上面`git stash`之后，工作区的修改被存储，然后工作区是干净的



首先确定在哪个分支修复Bug，假定需要在`master`分支上修复，就从`master`上创建临时分支

```bash
$ git checkout master

$ git checkout -b issue-101
```



修复完之后切换到`master`分支，并完成合并，最后删除分支（命令行如前面）



然后修复完Bug之后，需要恢复工作区

首先使用`git stash list`可以查看保存的工作现场

```bash
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

然后，使用`git stash apply`恢复，然后将`git stash drop`删除刚刚创建的工作现场

```bash
$ git stash apply
$ git stash drop
```

另一种方式是使用`gits stash pop`,恢复内容打得同时把原来的工作现场记录删除了

```bash
$ git stash pop
```



你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```bash
$ git stash apply stash@{0}
```



上面操作的是`master`分支，修复了bug，这时候`dev`分支还是原来的

这时候，我们可以使用`merge`，切换到`dev`，然后和`master`分支进行`merge`



也可以单独提交一个commit给`dev`，命令行如下：

```bash
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
```

如上，其实就是找到该commit的SHA1值，然后使用cheery-pick



总结：

1. 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

2. 当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；

3. 在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。







### Feature分支

开发一个新功能（feature），最好新建一个分支。在上面开发，完成后合并，最后再删除



如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除





### 多人协作



### Rebase



## 标签管理

### 创建标签

### 操作标签





## 问题

diff是什么

Unix的哲学是“没有消息就是好消息”