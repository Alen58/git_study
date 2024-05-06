链接：https://www.liaoxuefeng.com/wiki/896043488029600/898732864121440
##### 创建版本仓库

初始化一个git仓库，使用`git init`命令

```
// 选择一个合适地方创建一个空目录
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit

// 通过git init将这个空目录变成git可以管理的仓库
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

添加文件到Git仓库，分两步：

1. 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；

2. 使用命令`git commit -m <message>`，完成。 

   ```
   git add readme.txt
   git add readme_change.txt
   git commit -m "add 2 files"
   ```

   

查看仓库状态：git status

查看文件具体修改内容：git diff readme.txt



##### 版本回退

文件修改到某个程度的时候就可以commit一下，当后面不小心把文件改乱了，可以从最近的一个commit恢复

可以用`git log`命令查看提交的历史记录 git log --pretty=oneline表示将内容显示在一行上，如下，append GPL表示最近一次提交commit -m中的内容，109……一长串表示的是提交的commit id.

HEAD表示当前版本，即最新提交版本，上一个版本就是HEAD^,上上个版本就是HEAD^^,往上100个版本是HEAD~100

```
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

准备回退readme.txt到上一个版本add distributed

```
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

如果想要撤销回退，只要命令行窗口还未关闭能找到之前最新版本的commit id,即可返回

```
$ git reset --hard  1094a   // 1094a是commit id前几个字符

```

```ascii
┌────┐
│HEAD│
└────┘
   │
   └──▶ ○ append GPL
        │
        ○ add distributed
        │
        ○ wrote a readme file
```

HEAD指针改为指向`add distributed`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   │    ○ append GPL
   │    │
   └──▶ ○ add distributed
        │
        ○ wrote a readme file
```

总结

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

##### 工作区和暂存区

工作区：在电脑里能看到的目录

版本库：工作区中有一个隐藏目录.git，是git的版本库，不算工作区

> git版本库里存了很多东西，最重要的就是称为stage或index的暂存区、还有git自动创建的第一个分支master,以及指向master的一个指针HEAD
>
> ![git-repo](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)
>
> git add 即为将文件添加到暂存区
>
> git commit 即为将暂存区的所有内容提交到当前分支

##### 管理修改

git管理的是修改，而不是文件，比如说对一个文件修改了两次，只git add 第一次修改到暂存区，git commit的话就只提交第一次修改，第二次不提交

即为每次修改，如果不用git add到暂存区，那就不会加入到commit中

##### 撤销修改

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退，不过前提是没有推送到远程库。

##### 删除文件

先添加一个新文件到git并且提交到版本库。若该文件在工作区被删除了rm file，由于版本库中还有该文件，则可以使用git checkout -- test.txt恢复误删文件，若该文件在版本库被删除了git rm file，那就确实被删除了。

`git checkout` 其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以一键还原，但是<u>在上一版本之后修改的那些内容会丢失且无法恢复</u>

##### 远程仓库github

添加远程仓库

> 登录github，创建一个新的git仓库 名为learngit
>
> 将本地learngit仓库与这个新的仓库关联起来运行命令
>
> git remote add origin git@github.com:Alen58/learngit.git
>
> 添加后远程仓库的名字就是origin
>
> 将本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程
>
> git push -u origin master    -u参数将本地master分支和远程master关联起来
>
> 之后只要本地做了提交，就可以通过命令 git push origin master 把本地master分支的最新修改推送到github

删除远程仓库

> 先查看远程仓库信息
>
> git remote -v
>
> 然后根据名字删除，比如删除origin
>
> git remote rm origin

从远程仓库克隆

> git clone 远程仓库地址

##### 分支管理

###### 创建与合并分支

> ①创建并切换dev分支 原本为master分支
>
> git branch dev
>
> git checkout dev
>
> ②一条语句完成创建及切换分支 -b参数表示创建并切换
>
> git checkout -b dev   
>
> ③使用命令查看当前分支 为dev
>
> git branch
>
> ④在新的dev分支上对文件readme.txt做出修改并提交
>
> git add readme.txt
>
> git commit -m "branch test"
>
> ⑤切回master分支
>
> git checkout master
>
> ![git-br-on-master](https://www.liaoxuefeng.com/files/attachments/919022533080576/0)
>
> ⑥将dev分支的工作成果合并到master分支上 这种合并是快进模式--fast-forward，直接把master指向dev分支，合并好是一条线
>
> git merge dev   // git merge 用于合并指定分支到当前分支
>
> ⑦合并完成后就可以放心删除dev分支了
>
> git branch -d dev
>
> git branch命令查看branch，就只剩下master分支了
>
> git鼓励使用分支完成某个任务，合并后再删掉分支

###### 解决冲突

> ①创建一个新的分支并切换到该分支
>
> git branch -b feature
>
> ②在feature分支上对readme.txt文件进行修改并提交
>
> git add readme.txt
>
> git commit -m "AND simple"
>
> ③切换到master分支
>
> git branch master
>
> ④在master分支上对readme.txt文件进行修改并提交
>
> git add readme.txt
>
> git commit -m "& simple"
>
> 现在，master和feature分支各自都有新的提交，这种情况无法快速合并，只能试图把各自修改合并起来，但这种合并可能会有冲突
>
> ![image-20240430162800802](/home/dt060053/.config/Typora/typora-user-images/image-20240430162800802.png)
>
> ⑤合并出现冲突:将feature分支合并到当前master分支 
>
> git merge feature
>
> 直接查看readme.txt的内容
>
> ```
> Git is a distributed version control system.
> Git is free software distributed under the GPL.
> Git has a mutable index called stage.
> Git tracks changes of files.
> <<<<<<< HEAD
> Creating a new branch is quick & simple.   // 删除这个
> =======
> Creating a new branch is quick AND simple.   // 保留这个
> >>>>>>> feature1
> ```
>
> ⑥修改并解决冲突后再次提交(图中显示再次提交后分支情况)
>
> git add readme.txt
>
> git commit -m "conflict fixed"
>
> ![image-20240430164024189](/home/dt060053/.config/Typora/typora-user-images/image-20240430164024189.png)
>
> ⑦删除feature分支
>
> git branch -d feature
>
> ⑧使用git log --graph命令看分支合并图



###### 分支管理策略

> 不使用快进模式Fast forward合并分支，merge后就像这样（git log查看分支图）
>
> git merge --no-ff -m "merge with no-ff" dev
>
> ![image-20240430171445860](/home/dt060053/.config/Typora/typora-user-images/image-20240430171445860.png)
>
> 在实际开发中，我们应该按照几个基本原则进行分支管理：
>
> 首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
>
> 那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；
>
> 你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。
>
> 所以，团队合作的分支看起来就像这样：
>
> ![git-br-policy](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)



###### bug分支

> 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
>
> 当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；
>
> 在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit_id>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

###### feature分支

> 为了不把主分支搞乱，每添加一个新的功能，最好新建一个feature分支，在上面开发完成，合并到主分支，最后删除该feature分支
>
> 如果feature分支功能不需要了，可以通过git branch -D feature强行删除该分支

###### 多人协作

> git remote -v 显示远程仓库的信息

推送分支

> git push origin master  // 向远程origin推送master分支
>
> git push origin dev        // 向远程origin推送dev分支

多人协作的工作模式

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

###### Rebase

> git rebase
>
> - rebase操作可以把本地未push的分叉提交历史整理成直线；
> - rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

##### 标签管理

创建标签

> git tag v1.0  // 默认在当前分支下最新提交的commit上打标签为v1.0 
>
> git tag  // 查看所有标签
>
> git log --pretty=oneline --abbrev-commit  // 显示历史提交commit
>
> git tag v0.9 commit_id   // 在某次提交上打标签
>
> git tag -a 标签名字 -m "标签说明" commit_id   //创建带有说明的标签
>
> git show <tagname>   // 显示标签信息（包括说明）
>
> git push origin master --tags //将本地分支master上标签也推送到远程

操作标签

> - 命令`git push origin <tagname>`可以推送一个本地标签；
> - 命令`git push origin --tags`可以推送全部未推送过的本地标签；
> - 命令`git tag -d <tagname>`可以删除一个本地标签；
> - 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。