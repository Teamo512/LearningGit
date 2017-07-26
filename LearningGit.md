### Git基本概念
主要有三个模块：工作区、暂存区、版本库。
1. 工作区：就是你电脑上能看到的工作目录。
2. 暂存区：stage或index， 一般存放在工作目录的“.git“目录下index文件中，即".git/index"，也叫索引。
3. 版本库：是工作区的一个隐藏目录".git".
[![git](./git.jpg "git")](http://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg "git")

### git基本命令
##### 1. git init
初始化一个git仓库，进入需要的成为仓库的文件目录下，输入此命令，则会在该目录下生成一个隐藏文件.git目录。此时该工作目录就变为Git仓库。
##### 2.git init new_directory
即将指定的新目录new_directory设置成Git仓库。

### Git的工作是创建和保存项目的快照，并与之后的快照进行对比。

##### 3. git add
将文件添加到暂存区，git add .  表示将整个目录下的所有文件都添加到暂存区。git add file 表示将指定文件添加到暂存区。

#### 4. git status
查看当前项目的状态，显示的是你上次提交到暂存区后的修改或写入暂存区的变化。
添加参数 -s 表示仅获取简短信息

```
Administrator@Lab413_8 MINGW64 /f/GitWorkSpace/myGit (master)
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
        new file:   fist.txt
	new file:   second.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   second.txt


Administrator@Lab413_8 MINGW64 /f/GitWorkSpace/myGit (master)
$ git status -s
A  fist.txt
AM second.txt
```

在git status -s 中"AM"状态的意思是该文件在添加到暂存区后有了新的改动。

#### 5. git diff
是用来查看执行git status的结果的详细信息。
git diff显示的是已经添加到暂存区，与修改但未添加到暂存区的改动的区别。
git diff ：是工作区(work dict)和暂存区(stage)的比较
git diff --cached ：是暂存区(stage)和分支(master)的比较
git diff HEAD   ：查看工作区和版本库里面最新版本的区别


#### 6. git commit -m "modifyMessage"
提交暂存区到版本仓库，并附带提交信息。
可以将git add命令和这条命令融合：参数改为 -am，即：git commit -am "modifyMessage"。

> 但注意一点，-am这个参数表示的是对于已经stage状态的文件再次修改后直接add和commit合并操作。但对于没有添加到暂存区的文件，是无法进行-am一次性完成add和commit操作的。！！！

#### 7. git log
查看提交历史，即通过该命令可以查看以往的提交记录.当你回退到某个版本后，就只能查看这个版本之前的提交记录了，比如说，现在有四次提交，按时间近到远依次为A、B、C、D。当从A回退到C，那么git log只能查看到C、D的提交记录，而A、B的都看不到了。

#### 8. git reflog
该命令用于查看命令历史，记录每次的命令。即便回退到某个版本，仍然可以查看到位于此版本之后的版本提交记录号。

#### 9. git reset --hard HEAD^
HEAD^表示回退到最近一次提交的版本，即上一个版本。
HEAD^^ 表示回到上上个版本。
对于更多的版本，可用HEAD\~n代替，其中n表示要退回的版本数，比如HEAD\~45，表示退回到第45个版本。
对于，当按时间近到远依次为A、B、C、D四个版本，当退到C时，再想回到B，可以用git reflog查看B的B_commit_id，然后git reset --hard B_commit_id，这样就可以回到B。


### Git的跟踪与管理是基于修改，而非文件。
比如说，对文件A，首先我们修改一次，然后git add A， 这样A经修改后被添加到暂存区，然后commit提交到仓库，这时查看文件，可以看到文件的修改。但此时再对文件修改，之后直接commit到仓库，再次查看文件A，会发现第二次修改并没有在文件中出现，这是因为第二次修改并没有进行提交到暂存区，所以即便commit也没办法检测到其修改的痕迹。所以说Git只负责把暂存区的修改提交到仓库。
所综上，我们可以得出：如果每次修改后不add到暂存区，那就不会被加入到commit中。

#### 10. git checkout -- file
撤销工作区的全部修改。有两种情况：
1. file自修改后还没有添加到暂存区，现在撤销就直接回到和版本库里最后一次commit时一样的状态了。
2. file修改中有添加到暂存区，然后又有修改，但没有再add，现在撤销，则回到最后一次添加到暂存区时的状态。

总之，就是让文件回到最近一次commit或add时的状态。

#### 11. git rm
在Git中，删除也属于一种修改。
在Linux下，rm 删除的是工作区的文件；
git rm 删除暂存区文件；
> 在工作区，用rm删除文件，但此时还未将这个修改放到暂存区，然后用git rm命令，就是把工作区删除文件操作添加到暂存区。如果，直接用git rm命令，就相当于直接完成了rm 和 git rm两条命令，rm被自动执行。此时如果再执行commit命令，则即将缓存区的修改提交到版本库中，则版本库中相应文件也将被删除。


#### 12. git mv
git mv 命令用于移动或重名名一个文件，目录，软连接。
```
git mv [-v] [-f] [-n] [-k] source destination
git mv [-v] [-f] [-n] [-k] source destinationDirectory
```

第一种形式中，它将重命名source为destination，source必须存在，并且是文件，符号链接或目录。 在第二种形式中，最后一个参数必须是现有的目录; 给的源source将被移动到这个目录中。

> git mv test.txt hell.txt

运行此命令等同于运行:
1. mv test.txt hello.txt
2. git rm test.txt
3. git add hello.txt

这就表明重命名后的状态已经为stage状态。但此时版本库的状态仍为原始状态。


#### 13. git remote
管理一组跟踪的远程存储库。
1. git remote  
> 不带参数，表示列出所有已经存在的远程仓库。
2. git remote -v 
> 表示列出远程仓库的url地址详细信息。
3. git remote add 仓库名 仓库URL
> 表示添加一个新的远程仓库，同时指定一个简单的名字(随便起)。
比如：git remote add origin git@github.com:Teamo512/test.git


#### 14. git push
将本地分支的更新推送到远程仓库。
> git push 远程仓库名 本地分支名 **:** 远程分支名

1. git push origin master
> 表示将本地的master分支推送到远程仓库为origin的master分支上。如果远程master分支不存在，则会新建。

2. git push origin :master
> 即本地分支名为空，表示删除指定的远程分支，因为推送的是一个空的本地分支到远程分支。也就等同于：git push origin --delete master。

3. git push origin
> 表示将当前分支推送到origin仓库的对应分支，如果当前分支只有一个追踪分支，那么远程仓库名都可以省去：git push。

4. git push -u origin master
> 如果当前分支与多个仓库存在追踪关系，则参数 -u 表示指定一个默认仓库，第一次这样操作后，在后续的操作中，可以不用加任何参数，直接使用git push即可。

5. git push --all origin
> 表示将当前本地所有分支全部推送到origin远程仓库中。

总之，方法很多，可参考：[git push命令](http://www.yiibai.com/git/git_push.html "git push命令")


### 分支管理

#### 15. git branch branchName
创建分支，分支名为branchName。

#### 16. git checkout branchName
切换到分支branchName上。

#### 17. git checkout -b branchName
这条命令是上面两条命令的综合，即表示：创建并切换到分支branchName上。

**git checkout -b New_branchName branchName**

> 表示在branchName分支上创建新分支New_branchName，并切换到该新分支上。


#### 18. git branch
表示列出本地仓库的所有分支，且当前分支前面会有一个\*号。

**git branch -f**

> 参数: -f 表示查看拉取回本地的远程分支。

**git branch -a**
> 参数: -a 表示查看所有分支（包括本地分支和远程分支）

#### 19. git merge bak_branch
表示将指定分支bak_branch合并到当前分支上。这种合并模式是Fast-forward模式，“快进模式”， 直接把master指向bak_branch的当前提交，仅仅是指针的移动，所以合并速度非常快。


#### 20. git branch -d branchName
删除分支，进行完合并操作，将其他分支合并到master上后，可以删除不用的分支，便可以执行此命令。

#### 21. git merge --no-ff -m "message" bak_branch
不采用fast-forward模式进行合并，这样需需要重新commit一次，所以有-m参数，且这样提交后可以通过历史知道曾经做过合并。

### 标签管理
> tag 用于创建一个标签 用于在开发阶段，某个阶段的完成，创建一个版本，在开发中都会使用到, 可以创建一个tag来指向软件开发中的一个关键时期，比如版本号更新的时候可以建一个version1.0, version1.2之类的标签，这样在以后回顾的时候会比较方便。

#### 22. git tag tagName
创建一个新标签，此时没有指定是在哪个commitID上打标签，所以默认的标签是搭载最新的提交的commitID上。

#### 23. git tag tagName commitID
根据相应的commitID来创建标签。

#### 24. git tag -a tagName -m “message”
创建一个含有说明信息的标签。-a 指定标签名，-m指定说明信息。

#### 25. git tag
不带任何参数，该命令即查看所有标签，只是简单列出标签名，且按字母序，并不是按标签建立的时间序。

#### 26. git show tagName
查看指定标签的详细信息。

#### 27. git tag -s tagName -m "message"
-s 表示通过GPG私钥加密标签。
GPG的安装，简介，具体操作，参考：
- [在Windows系统使用Gpg4win进行加密解密](http://blog.csdn.net/u014076884/article/details/46498239 "在Windows系统使用Gpg4win进行加密解密")
- [带GPG签名的Git tag](http://airk000.github.io/git/2013/09/30/git-tag-with-gpg-key "带GPG签名的Git tag")
- [gpg: skipped "xxx": secret key not available的一种解决方法](http://blog.csdn.net/sh21_/article/details/71082422 " gpg: skipped "xxx": secret key not available的一种解决方法")
- [GPG入门教程](http://www.ruanyifeng.com/blog/2013/07/gpg.html "GPG入门教程")

#### 28. git tag -d tagName
删除标签。这是删除本地的标签。

#### 29. git push origin tagName
将标签推送到远程仓库origin上。
git push origin --tags
一次性将所有标签都推送到远程仓库。

#### 30. git push origin :refs/tags/tagName
首先需从本地删除指定标签，然后再删除远程仓库的指定标签。

#### .gitignore文件
工作区中不想被提交的文件。将所有不想被跟踪的文件名写到.gitignore中。
> 如果.gitignore中一个规则表示某一类文件将不会被跟踪，但现在有一个该类文件希望被跟踪，比如，.gitignore文件中有这样一条规则：\*.class即表示以class为后缀的额文件都将被忽略。但现在有个A.class文件希望被跟踪。则可以使用-f参数强制添加。**git add -f A.class**。
**git check-ignore -v A.class** 这条命令可以查看是.gitignore文件中那条规则导致A.class文件被忽视。

#### 31. git fetch origin
从一个远程仓库origin中拉取分支或标签等对象。拉取到本地的.git/refs/目录中。所有分支存储在.git/refs/remotes/origin/目录下，所有标签存储正在.git/refs/tags/目录中。
**git fetch 远程仓库 分支**
该命令从远程仓库中拉取指定分支到本地仓库，且存在.git/refs/remotes/目录下。


#### 32. git pull命令
该命令用于从一个仓库获取分支并与当前仓库合并。
等同于一下两条命令的缩写：
1. git fetch
2. git merge

- **git pull 远程仓库 远程分支 ： 本地分支**
表示拉取回远程仓库的指定分支和本地指定的分支合并。如：**git pull origin master:master**
- **git pull 远程仓库 远程分支**
表示拉取回远程仓库的指定分支和本地当前分支合并。如本地当前分支为master，则命令：**git pull origin bak**就是将仓库origin的bak分支与本地当前的分支master合并。

具体参考：[git pull](http://www.yiibai.com/git/git_pull.html "git pull")


---
以上是本人初学Git时，边学边整理的常用命令，仅是文字描述。学习Git作重要的还是多操作，多敲命令。
本笔记，比较简单，并不详细，希望以后慢慢更新吧！

参考文献：

- [廖雪峰Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000 "廖雪峰Git教程")
- [RUNOOB.com Git教程](http://www.runoob.com/git/git-tutorial.html "RUNOOB.com Git教程")
- [易百教程——Git教程](http://www.yiibai.com/git/ "易百教程——Git教程")
