# git：分布式版本控制系统

版本库(仓库)：里面所有文件都可被git管理，每个增删查改都可追踪，也可还原.

- `$ git add xx`：添加xx文件到暂存区；
- `$ git commit -m 'aa'`：提交到版本库，附上描述aa；
- `$ git status`：查看仓库当前状态(看看有没有需要提交的)；
- `$ git diff xx`：查看xx文件每次修改的不同(提交后才可比较)；
- `$ git log`：查看提交历史记录(里面有版本号，方便退回到哪个文件)；
- `$ git reset --hard HEAD^`：退回上次修改版本；
- `cat xx`：查看内容(判断xx是否退回到上次修改)；
- `$ git reset --hard 11111`：退回到版本号开头为11111到版本；
- `$ git reflog`：记录每次命令历史(可查看版本号，防止上面方法找不到版本号)；
- `$ git checkout -- xx`：撤销工作区修改；
  - 未`add`，回到版本库最新；
  - 已`add`，先撤销`add`：`$ git reset HEAD xx`，再用1；
  - 已`commit`，版本退回；
- `$ rm xx`：直接在工作区删除(前提已提交，未提交可直接用上面撤回，此时工作区和版本库不一致，所以要看是否要删除版本库还是要撤销这次删除)；
  - `$ git rm xx`：删除版本库，并附上说明：` $ git commit -m "aa"`；
  - `$ git checkout -- xx`：撤销；
- `$ git checkout -b dev`：创建并切换到分支dev；
- `$ git branch`：查看当前分支；
- `$ git checkout master`：切回主分支；
- `$ git merge dev`：合并dev到主分支(前提当前位于主分支，且dev的修改已提交)；
- `$ git branch -d dev `：删除分支；
- `$ git stash`：储存当前工作；
- `$ git stash pop`：恢复；
- `$ git branch -D dev`：强行删除(未合并分支dev分支就不要了)；
- `$ git remote -v`：查看远程库信息；
- `$ git pull`：抓取远程新提交；
- `$ git push origin master`：推送master；



## 修改提交文件：

```python
$ mkdir learngit  # 新建一个learngit文件夹
$ cd learngit     # 打开文件夹
$ pwd							# 查看当前目录
/Users/michael/learngit

$ git init    # 把这个目录变成git可管理的仓库(git初始化)
Initialized empty Git repository in /Users/michael/learngit/.git/  # .git不要乱动

(vi readme.txt)# i开始写，esc+:wq退出
# 编写一个readme.txt文件
Git is a version control system.
Git is free software.

# 文件要放到learngit子目录下,
$ git add readme.txt   # 添加文件到暂存区(stage)
 # -m后面输入对本次提交的描述，执行成功会提示：1 file changed ;2 insertions : 插入了两行内容
$ git commit -m "wrote a readme file"   # 一次性提交暂存区所有修改

# 可以一次提交很多文件
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."

# 修改readme.txt文件
Git is a distributed version control system.
Git is free software.

$ git status  # 掌握仓库目前状态
On branch master
Changes not staged for commit:  # readme.txt被修改但未提交
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

$ git diff readme.txt  # 查看修改内容(查看不同)
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
  
# 提交修改
$ git add readme.txt

$ git status  # 查看状态
On branch master
Changes to be committed:  # 表示将要被提交的修改包括readme.txt
  (use "git reset HEAD <file>..." to unstage)

	modified:   readme.txt
    
$ git commit -m "add distributed"  # 提交
[master e475afc] add distributed
 1 file changed, 1 insertion(+), 1 deletion(-)
  
$ git status   # 此时没有需要提交的
On branch master
nothing to commit, working tree clean
```

```python
# 再修改一次readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.

$ git add readme.txt
$ git commit -m "append GPL"

$ git log  # 查看提交历史记录
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
# 输出太多想简短查看
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

## 版本回退：

Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写成`HEAD~100`

```python
$ git reset --hard HEAD^  # 回退到上次修改
HEAD is now at e475afc add distributed

$ cat readme.txt  # 查看内容是否退回成功
Git is a distributed version control system.
Git is free software.

$ git log    # 此时最新版本消失，怎么办，要回到最新版本
commit e475afc93c209a690c39c13a46716e8fa000c366 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
# 在没关闭上面命令窗口情况下  
$ git reset --hard 1094a   # 找到最新版本版本号，写前几位，自动配对
HEAD is now at 83b0afe append GPL

$ cat readme.txt   # 此时查看内容，又是最新版本了
Git is a distributed version control system.
Git is free software distributed under the GPL.

# 关掉命令窗口后怎么回到最新版本，即怎么找版本号
$ git reflog    # 记录每次命令历史，即可查看最新版本版本号
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

![屏幕快照 2019-07-23 08 53 47](https://user-images.githubusercontent.com/53204023/71946162-fbdbd400-3203-11ea-8e20-0008a1eba14c.png)

```python
# 再次修改readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
# 然后，在工作区新增一个LICENSE文本文件（内容随便写）
$ git status
On branch master
Changes not staged for commit:  # 告诉我们readme被修改
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Untracked files:    # license还没被添加
  (use "git add <file>..." to include in what will be committed)

	LICENSE

no changes added to commit (use "git add" and/or "git commit -a")
$ git add readme.txt
$ git add LICENSE
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   LICENSE
	modified:   readme.txt
```

![屏幕快照 2019-07-23 08 53 47](https://user-images.githubusercontent.com/53204023/71946146-f1213f00-3203-11ea-8428-87c6f549c48c.png)

再commit后变为：

![屏幕快照 2019-07-23 08 53 59](https://user-images.githubusercontent.com/53204023/71946150-f3839900-3203-11ea-9202-26ea44a54a57.png)

此时暂存区为空。

工作区----add(暂存区)----commit(版本库)

## 管理修改：

git跟踪管理的是修改而非文件

```python
# 仍然修改readme为
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
$ git add readme.txt   
$ git status
# 此时修改被添加到暂存区，等待提交

# 再修改
$ cat readme.txt 
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
$ git commit -m "git tracks changes"   # 此时因为第二次还没被添加到暂存区，所以提交的只是第一次的修改
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
$ git diff HEAD -- readme.txt   # 提交后查看不同
diff --git a/readme.txt b/readme.txt
index 76d770f..a9c5755 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,4 +1,4 @@    # 此处就是第一个修改和第二次修改的区别
 Git is a distributed version control system.
 Git is free software distributed under the GPL.
 Git has a mutable index called stage.
-Git tracks changes.
+Git tracks changes of files.  
```

## 撤销修改：

```python
# 假设现在修改了一个错误readme，在提交之前发现错误，怎么撤回
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
My stupid boss still prefers SVN.
$ git status  # 先查看当前状态
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
# 此处告诉我们可丢弃修改，火速使用
	modified:   readme.txt
no changes added to commit (use "git add" and/or "git commit -a")

$ git checkout -- readme.txt  # 把readme在工作区的修改全部撤销
# readme未add,即未放到暂存区，撤销相当于回到和已提交的版本库一样状态
# readme已add,又修改，撤销回到添加暂存区状态  
# 总之，就是让这个文件回到最近一次git commit或git add时的状态
$ cat readme.txt   # 再查看已撤销成功，此时是第一种情况
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.

# 第二种情况，已add
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
My stupid boss still prefers SVN.
$ git add readme.txt
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
# 此处告诉我们可撤销add
	modified:   readme.txt
$ git reset HEAD readme.txt  # head表示退到最新版本
Unstaged changes after reset:
M	readme.txt
$ git status   # 此时退回到未add
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt
# 然后用第一种方法
$ git checkout -- readme.txt

# 第三种，已commit，此时用版本退回，前提没有推送到远程库
```

## 删除文件

```python
$ git add test.txt
$ git commit -m "add test.txt"
$ rm test.txt  # 直接在文件管理器删除(工作区)

# 第一种情况，确定要从版本库删除(和add对应功能一样),如果一个文件已经被提交到版本库，那么永远不用担心误删，但是要小心，只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。
$ git rm test.txt  
$ git commit -m "remove test.txt" # 删除后要提交说明
# 第二种情况，删错了
$ git checkout -- test.txt
# 第三种情况，没有commit的文件删除后无法恢复
```

## 远程仓库

## 分支管理

```python
# 创建dev分支，并切换到dev分支，-b 表示切换
$ git checkout -b dev   
Switched to a new branch 'dev'
# 等价于
$ git branch dev   # 创建分支
$ git checkout dev # 切换分支

$ git branch  # 查看当前分支，会列出所有分支   加-a查看隐藏分支
* dev    # 当前分支前会有*
  master

# 在dev分支修改提交
Creating a new branch is quick.  # 给readme加一行(vi readme.txt)
$ git add readme.txt 
$ git commit -m "branch test"
# 此时dev分支工作完成，需要切回master

$ git checkout master   # 切回master，此时master上没有刚刚修改的文件
# 现在把dev分支成果合并到master
$ git merge dev  # 合并指定分支到当前分支
Updating d46f35e..b17d20e
Fast-forward  # 快进模式，合并速度很快
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
# 合并后可放心删除dev分支
$ git branch -d dev   # 删除分支
```

## 解决冲突

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

```python
# 创建一个新分支
$ git checkout -b feature1
Creating a new branch is quick AND simple. # 修改readme
$ git add readme.txt
$ git commit -m "AND simple"  # 提交
$ git checkout master # 切换master
Creating a new branch is quick & simple. # 在当前master修改文件
$ git add readme.txt 
$ git commit -m "& simple" # 也提交
# 此时分支feature1和master都有新的提交
$ git merge feature1  # 合并到当前分支(master)冲突
$ cat readme.txt # 查看内容
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick and simple. # 再修改文件为
$ git add readme.txt 
$ git commit -m "conflict fixed"  # 再提交
$ git branch -d feature1 # 删除分支
```

## 分支管理策略

master分支应该非常稳定，不能在上面干活；干活都在dev上，

```python
$ git checkout -b dev  # 重建切换dev
$ git add readme.txt 
$ git commit -m "add merge"  # 修改文件提交
$ git checkout master  # 切回master
# --no-f表示禁用fast forward，因为本次合并要创建一个新的commit，所以加上-m，描述
$ git merge --no-ff -m "merge with no-ff" dev
# 能通过历史看出做过合并，ff看不出来
$ git log --graph --pretty=oneline --abbrev-commit  # 查看分支历史
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
```

## bug分支

当前dev上进行的工作还没有提交，因为未完成无法提交，但此时必须修复一个bug101

```python
$ git stash  # 把当前工作现场储存起来，之后继续工作
# 首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支
$ git checkout master
$ git checkout -b issue-101   # 创建临时分支
# 修改bug
$ git add readme.txt 
$ git commit -m "fix bug 101" # 提交
$ git checkout master  # 修复完成后返回master，合并，删除
$ git merge --no-ff -m "merged bug fix 101" issue-101  # 合并
$ git branch -d issue-101 # 删除分支

# 现在回去继续干活
$ git checkout dev
$ git stash pop  # 恢复
# 另一种方法
$ git stash list   # 查看刚刚工作存到哪里
stash@{0}: WIP on dev: f52c633 add merge
$ git stash apply stash@{0}
# 用此法恢复，需要用 git stash drop 删除stash内容
```

## feature分支

开发新功能不想搞乱主分支代码

```python
$ git checkout -b feature-vulcan  # 准备开发
$ git add vulcan.py
$ git commit -m "add feature vulcan" # 开发完毕提交
$ git checkout dev  # 切回dev准备合并，其后步骤和bug一样
# 但此刻命令新功能又不要了，必须就地销毁
$ git branch -D feature-vulcan  # 强行删除
```

## 多人协作

从远程仓库克隆时，git自动把本地的`master`分支和远程的`master`分支对应起来了，远程仓库的默认名称是`origin`.

```python
$ git remote  # 查看远程库信息
origin
$ git remote -v # 更详细信息
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
```

### 推送分支

把该分支上的所有本地提交推送到远程库，推送时要指定本地分支，这样，git就会把该分支推送到远程库对应的远程分支上：

```python
$ git push origin master   # 推送master
$ git push origin dev      # 推送dev
```

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

### 抓取分支

模拟一个你的小伙伴，可以在另一台电脑（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆：

```python
$ git clone git@github.com:michaelliao/learngit.git
Cloning into 'learngit'...
remote: Counting objects: 40, done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 40 (delta 14), reused 40 (delta 14), pack-reused 0
Receiving objects: 100% (40/40), done.
Resolving deltas: 100% (14/14), done.
```

默认情况下，你的小伙伴只能看到本地的`master`分支

```python
$ git branch
* master
```

你的小伙伴要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，于是他用这个命令创建本地`dev`分支：

```python
$ git checkout -b dev origin/dev
# git checkout -b dev//基于本地创建分支 
# git checkout -b dev origin/dev //基于远程分支创建本地分支
```

他就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程：

```python
$ git add env.txt
$ git commit -m "add env"
$ git push origin dev
```

小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：

```python
$ cat env.txt
env
$ git add env.txt
$ git commit -m "add new env"
$ git push origin dev  # 失败，冲突
```

解决办法：先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

```python
$ git pull  # 也失败了，没有指定本地dev分支与远程origin/dev分支的链接
$ git branch --set-upstream-to=origin/dev dev  # 设置dev和origin/dev的链接
$ git pull  # 成功
# 此时合并有冲突，看解决冲突
$ git commit -m "fix env conflict"  # 解决后提交
$ git push origin dev   # push成功
```

多人协作的工作模式通常是这样：

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

### issue
拉取到本地的文件里有`HEAD`分支，创建分支会报错，可以删除这个分支
```shell
git branch -D HEAD
```
