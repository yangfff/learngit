# 一.创建版本库 #
> $ mkdir learngit// 创建文件夹learngit
> 
> $ cd learngit
> 
> $ pwd//显示当前路径

①初始化一个Git仓库：
	
	git init//将当前目录变成一个Git可以管理的仓库

②添加文件到Git仓库：

	1.git add<file> ;  2.git commit
	

>$ git add readme.txt // 将文件添加到Git仓库（把文件修改添加到暂存区）
>
>$ git add file1.txt // 添加file1.txt文件

>$ git add file2.txt file3.txt // 同时添加file2.txt和file3.txt两个文件

>$ git commit -m "wrote a readme.txt." // 将文件提交到仓库（把暂存区的所有内容提交到当前分支）

#  二.时光机穿梭 #
①查看工作区状态，文件是否被修改过：

	git status

②查看修改的内容：

	git diff
	
>eg:$ git diff readme.txt // 查看工作区的readme.txt与缓存区的readme.txt的区别

## 1.版本回退 ##
	①HEAD:当前版本

	②HEAD^:上个版本

	③定位版本：git reset --hard commit_id

	④git log：穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本

	⑤git reflog：要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
>
>$ git log // 查看最近到最远的提交记录（详情: commit id + Author + Date + comment）

>$ git log --pretty=oneline // 查看最近到最远的提交记录（简写：commit id + comment）

>$ git reset --hard HEAD^ // 回到上一个版本（HEAD: 当前版本，HEAD^: 上一个版本，HEAD~100: 往上100个版本）

>$ git reset --hard 1234567 // 回到指定版本号commit id（此处：commit id 假设为1234567******，Git会根据commit id的前几位自动寻找对应的版本）

>$ cat readme.txt // 查看readme.txt的内容

>$ git reflog // 查看每一次命令记录历史，确保能回到任意版本　　

## 2.工作区和暂存区 ##
①工作区：就是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区

②版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

③暂存区：Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区。 
          

	$ git diff readme.txt // 比较工作区（working directory）和暂存区（stage/index）的区别

	$ git diff --cached // 比较暂存区（stage/index）和分支（master）的区别



第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

## 3.管理修改 ##
①每次修改，如果不add到暂存区，就不会加入到commit中

## 4.撤销修改 ##
	①git checkout -- file：丢弃工作区的修改

	②git reset HEAD file：把暂存区的修改撤销掉，重新放回工作区

## 5.删除文件 ##
	①git rm：从版本库中删除文件
>$ rm test.txt // 删除工作区文件（类似于手动删除）

>$ git status // 查看当前工作区与缓存区状态
>
>$ git rm test.txt // 情况1：确认删除

>$ git commit -m "remove test.txt" // 情况1：确认删除后，提交到版本库

>$ git checkout -- readme.txt // 情况2：误删，需要回退（即：用版本库里的版本替换工作区的版本）

# 三.远程仓库 #
## 1.添加远程库 ##
①关联一个远程库：
	
	git remote add origin git@server-name:path/repo-name.git// 关联一个远程仓库
>eg:git remote add origin git@github.com:yangfff/learngit.git

②关联后第一次推送master分支的所有内容：
	
	git push -u origin master

③此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改
	
	git push -u origin master // 第一次推送master分支的所有内容

ps:由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。




注：在GitHub上创建新仓库时，如果勾选了README.md选项时，可能会出现下面错误，提示：远程仓库有readme.txt,而本地仓库没有README.txt,此时应该先进行合并文件，再进行推送。

	git pull --rebase origin master // 推送之前，进行合并


合并文件之后，发现本地仓库中多了README.md文件，此时再进行推送修改到远程仓库就可以了。
 

	再次执行：git push -u origin master, 即可推送本地仓库到远程仓库了
查看GitHub上的文件，已经更新！


## 2.从远程库克隆 ##
①要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆

②Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快
	
	$ git clone git@github.com:yangfff/learngit.git // 以SSH方式克隆

	$ git clone https://github.com/yangfff/learngit.git // 以Https协议方式克隆

# 四.分支管理 #
## 1.创建与合并分支 ##
	查看分支：	git branch

	创建分支：	git branch<name>

	切换分支：	git cheakout<name>

	创建+切换分支：	git cheakout -b <name>

	合并某分支到当前分支：git merge<name>

	删除分支：git branch - d <name>

## 2.解决冲突 ##
①查看分支合并图：

	git log --graph // 查看分支合并图

	git log --graph --pretty=oneline --abbrev-commit // 查看分支合并缩略图

## 3.分支管理策略 ##
①合并分支时加--no-ff参数：普通模式合并，合并后的历史有分支，禁用fast forward

eg:git log --no-ff-m"merge with no--ff"dev

## 4.Bug分支 ##
	
	
	git stash // 隐藏分支工作现场，为修复bug准备

	git stash list // 查看有哪些分支隐藏的工作现场，为恢复工作现场做准备

	git stash apply // 恢复工作现场，但不删除存储的stash内容，结合git stash drop进行删除

	git stash drop // 删除存储的stash内容，恢复到隐藏前的工作现场

	git stash pop // 恢复到隐藏前的工作现场，相当于git stash apply和git stash drop

	git stash apply stash@{0} // 可以多次stash，通过git stash list查看所有的stash，然后可以恢复到指定的隐藏的工作现场

## 5.Feature分支 ##
①开发一个新feature，最好新建一个分支；

②如果要丢弃一个没有被合并过的分支，可以通过
	
	git branch -D <name>//强行删除

## 6.多人协作 ##
①git remote -v：查看远程库信息

②git push origin branch-name：从本地推送分支

③git pull：推送失败时，抓取远程的新提交

④git checkout -b branch-name origin/branch-name：在本地创建和远程分支对应的分支（本地和远程分支的名称最好一致）

⑤git branch --set-upstream branch-name origin/branch-name：建立本地分支和远程分支的关联

# 五.标签管理 #
## 1.创建标签 ##
①git tag <name>：新建一个标签（默认为HEAD，也可以指定commit id）

②git tag -a <tagname> -m "blablabla..."：可以指定标签信息

③git tag -s <tagname> -m "blablabla..."：可以用PGP签名信息

④git tag：查看所有标签

## 2.操作标签 ##
①git push origin <tagname>：推送一个本地标签

②git push origin --tags：推送全部未推送过的本地标签

③git tag -d <tagname>：删除一个本地标签

④git push origin :refs/tags/<tagname>：删除一个远程标签

# 六.使用GitHub #
 

①在GitHub上，可以任意Fork开源仓库；

②自己拥有Fork后的仓库的读写权限；

③可以推送pull request给官方仓库来贡献代码。

# 七.自定义Git #
## 1.忽略特殊文件 ##
①忽略某些文件时，需要编写.gitignore；

## 2.配置别名 ##
①我们只需要敲一行命令，告诉Git，以后st就表示status：

	git config --global alias.st status

>eg:

>$ git config --global alias.co checkout

>$ git config --global alias.ci commit

>$ git config --global alias.br branch

>$ git config --global alias.unstage 'reset HEAD'

>$ git config --global alias.last 'log -1'

>git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
## 3.搭建Git服务器 ##
GitHub就是一个免费托管开源代码的远程仓库。但是对于某些视源代码如生命的商业公司来说，既不想公开源代码，又舍不得给GitHub交保护费，那就只能自己搭建一台Git服务器作为私有仓库使用。

例如大众点评code.dianpingoa.com