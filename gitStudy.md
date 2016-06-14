git 进阶
===

### 一.明确几个概念

* 工作区       [修改了但未add]
* 暂存区       [修改并且add了]
* 本次分支仓库  [commit]
* 远程分支仓库  [push]

工作区 -->git add file -->暂存区 -->git commit -m "xxx" -->本地分支-->git push origin master-->远程分支

* Git跟踪并管理的是修改，而非文件

### 二.流程&规范
[Git 使用规范流程](http://www.ruanyifeng.com/blog/2015/08/git-use-process.html)
[Git分支管理策略](http://www.ruanyifeng.com/blog/2012/07/git.html)
[Git flow工作流程](http://www.ruanyifeng.com/blog/2015/12/git-workflow.html)

### 三. 文件操作
checkout    reset
① 修改后 未add（添加到暂存区） 需要撤销修改时：
        git checkout -- myfile.txt 或 手动删除工作区修改
        工作区 ： clean  暂存区： clean
② 修改后 add了（未commit） 再次修改文件  要撤销第二次修改时：
        git checkout -- myfile.txt (将暂存区恢复到工作区)
        暂存区有第一次的修改需要commit
③ 修改后 add了（未commit），需要撤销修改时：
        git reset HEAD myfile.txt (将暂存区修改删除)
        此时工作区的修改还未撤销
        git checkout -- myfile.txt (撤销工作区修改)
④ 修改后 add并commit了，需要撤销修改时：
        git reset --hard HEAD^  (版本回退)**
注意：
git reset --hard会回退到你最近提交的版本上，如果你修改了工作区的内容只add了没有commit  那么使用了这个命令之后会回退到你最近一次commit的那个版本，你在工作区的修改就会丢失掉

删除文件 以及 恢复删除的文件。 git rm file  ;

###  四. 分支操作
(1) 拉取分支：
git clone xxx;git自动把本地master和远程master分支对应起来，并且远程仓库的名字默认为origin。可以通过git remote 或者git remote -v 来查看。
(2) 推送分支：
git push origin xxx;把本地xxx分支推送到远程服务器。如果远程存在xxx分支，就是同步内容；如果远程没有xxx分支，就是把本地分支push到远程，并且建立关联。
(3) 更新本地分支
git pull

(4) 删除分支
4.1 删除本地分支 在新分支开发功能完毕，如果功能需要，则merge到相应的分支，然后新分支可以通过 git branch -d new_branch 删除。
如果功能开发后，不需要了，即没有merge到其他分支，还用上面的删除命令就会报new_branch分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用命令git branch -D new_branch。即用大写的D强行删除

4.2 删除远程分支
```
   git push origin :分支名
   git push origin :<remote-branch> 或者 git push origin --delete <remote-branch>
   ```
4.3 清除已被远程分支删除的本地残余 git remote prune origin

（5）与主干同步
分支的开发过程中，要经常与主干同步
git fetch origin
git rebase origin/master

### 五. 别名提高效率
* git config --global alias.st status
* git config --global alias.co checkout
* git config --global alias.ci commit
* git config --global alias.br branch
* git config --global alias.unstage 'reset HEAD'
* git config --global alias.last 'log -1'
* git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
* git config --global alias.ref 'remote prune origin'

* git config --global alias.xxx xxxxxxx或者 git config --global alias.xxx 'xxxxxxxxxx'

### 六. 用好下面几个命令

####6.1. rebase
rebase 是一个很清爽的命令
$ git rebase <[origin/]branch>
rebase 用于重整分支到参数branch的下游，使gitflow看着十分清爽
进阶：git rebase -i 可以重排 commit 历史
缺点：
rebase 解决冲突较为繁琐
警告：
rebase 时请认准上游
只能用 rebase 重整本地提交

另外：git rebase 还可以把多次提交合并为一个[确保没有冲突]
git rebase -i HEAD~[number_of_commits]

####6.2. merge
merge 进阶：
 --squash 可以把 commits 合而为一
 --no-ff 可以禁止 fast-forward 从而产生 merge info

git merge --squash的使用场景&使用方法
feature分支修改提交了多次即多个commit，dev分支 merge feature分支时候，如果feature分支的commit messages是有意义的就保留多次commit信息，否则就使用
--squash进行合并，然后添加新的message进行提交


git merge --no-ff的使用场景&使用方法
feature分支修改提交了多次，即多个commit，dev分支 merge feature分支时，fast-forward方式就是当条件允许的时候，git直接把HEAD指针指向合并分支的头，完成合并。属于“快进方式”，不过这种情况如果删除分支，则会丢失分支信息。因为在这个过程中没有创建commit

问：什么时候用git rebase ，又是什么时候用git merge？
同一分支：git pull --rebase
不同分支：如果没有push到远程分支过，git rebase 上游branch.否则 git merge 上游分支。

####6.3. stash 
用于bug分支，当有临时事情需要处理，而手上需求分支尚未完成。可以通过stash，把当前修改的内容藏匿起来。在有bug的分支 checkout一个bug分支，处理完成之后，在回到需求分支，通过stash pop 把藏匿的内容恢复，继续工作
git stash
git stash list
git stash apply
git stash apply@{0}
git stash drop
git stash pop

注意：如果在git stash 后，不小心git stash pop 或者git stash drop ，也就是说stash的内容被丢弃了，但这些内容有有用，是否有办法恢复stash pop/drop的内容呐？

(1) 如果命令窗口尚未关闭，可以找到对应的stash pop/drop的stash_hash。然后通过 git stash apply stash_hash恢复。参考[How to recover a dropped stash in Git?](http://stackoverflow.com/questions/89332/how-to-recover-a-dropped-stash-in-git/7844566#7844566)
(2) 如果执行stash pop/drop 的窗口关闭了，通过 git fsck --no-reflog | awk '/dangling commit/ {print $3}' 或者 gitk --all $( git fsck --no-reflog | awk '/dangling commit/ {print $3}' ) 找到响应的stash_hash ,然后git stash apply stash_hash
参考[How to recover a dropped stash in Git?](7bf8709bb275632b584b994f8a32a91219501f19)
另外：
如果不确定就用 git stash apply。
如果本分支stash了内容，基于本分支 checkout -b 一个新分支也带有stash信息，并且这份stash信息是同一份，如果在新分支stash pop/drop了，之前的分支同样也不见了。

####6.4.cherry-pick
```
如果不想一次合并整个分支，那么…  cherry-pick 可以选择合并部分 commit
 在本地 master 分支上做了一个commit ( 38361a68138140827b31b72f8bbfd88b3705d77a ) ， 如何把它放到 本地 old_cc 分支上？

办法之一： 使用 cherry-pick.  根据git 文档：
Apply the changes introduced by some existing commits
就是对已经存在的commit 进行apply (可以理解为再次提交）
简单用法：
git cherry-pick <commit id>
如果顺利，就会正常提交
如果在cherry-pick 的过程中出现了冲突,就跟普通的冲突一样，手工解决,然后根据提示continue
通过git status查看状态
```

####6.5. reflog

```
你提交了一个你不想要提交的代码，最后你通过使用硬重置(hard reset)使其回到了之前的状态。稍后，你意识到，在这个过程中你丢失了一些其他的信息，并想要退回或是至少能看一眼。git reflog命令可以帮你做到这一点。

一个简单的git log命令，显示你最近的提交信息，以及上一次，再上一次的提交信息，以此类推。

而git reflog显示的是所有head移动的信息。记住，它是在本地的，而不是你仓库的一部分，不会包含在推送(push)和合并中(merge)
```




####6.6.  show

git fsck --lost-found
这里你可以看到丢失的提交，你可以使用git show [commit_hash]来查看这些提交所包含的改动或者是使用git merge [commit_hash]来恢复它。
git fsck比reglog有一个优势。比如你删除了一个远端分支并且克隆了仓库，使用fsck命令你可以搜索并恢复该远端分支


####6.7.  tag
##### tag是什么
tag就是标签，git也有在历史状态的关键点“贴标签”的功能--一般人们用这个功能来标记发布点

##### 如何打tag
git tag <name> 用于创建一个标签，默认为HEAD，也可以指定commitid
git tag -a <tagname> -m "taginfo" ；可以指定标签信息
git tag 查看所有标签
git push origin <tagname>可以推送一个本地标签
git push origin --tags可以推送全部未推送过的本地标签
git tag -d <tagname>可以删除一个本地标签
git push origin :refs/tags/<tagname>可以删除一个远程标签

##### 如何使用tag
先 git clone 整个仓库，然后 git checkout tag_name 就可以取得 tag 对应的代码了。

但是这时候 git 可能会提示你当前处于一个 detached HEAD 状态，因为 tag 相当于是一个快照，是不能更改它的代码的，如果要在 tag 代码的基础上做修改，你需要一个分支：git checkout -b branch_name tag_name

####6.8. revert回滚一个分支的合并

### 七. 和ide的结合
sourcetree
smartgit
androidstudio
等

### 八. 解决冲突
####8.1 产生冲突的原因
git pull 或者 git merge 或者 git rebase 或者 git stash pop的时候可能冲突
git 亦使用 <<<< ==== >>>> 格式的标示
####8.2 如何解决冲突
git st  git diff
结合ide使用比如android开发，结合androidstudio效率更高


### 九. 参考
4. [廖雪峰git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
5. [《Git Community Book 中文版》](http://gitbook.liuhui998.com/index.html)
6. [《Pro Git 中文版》]( http://git-scm.com/book/zh/)
7. [Getting Started Git Flow](http://yakiloo.com/getting-started-git-flow/)
8. [让你的Git水平更上一层楼的10个小贴士](http://blog.jobbole.com/75348/)
