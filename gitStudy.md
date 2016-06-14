如何设置简化键 比如 git commit --> git cm
git checkout --> git co

git status---> git st  用来查看发送变动的文件

1. git push origin master
remote: Permission to ayyb1988/android.git denied to starmier.
fatal: unable to access 'https://github.com/ayyb1988/android.git/': The requested URL returned error: 403


2. Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
说明：如果修改了内容，但是还没有提交到本地仓库，即没有staged。此时可以通过
git checkout -- <file> ... 进行删除本地未staged的修改。即红色部分


3. Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   README.md
说明：如果执行了add ，把修改添加到stage，此时如果不想要本次修改，可以通过git reset HEAD <file> ...来把stage仓库中的内容删除掉


4. git commit -m "change"
On branch master
Changes not staged for commit:
	modified:   README.md
	modified:   READMEBak.md

no changes added to commit

5. 与主干同步
分支的开发过程中，要经常与主干同步
git fetch origin
git rebase origin/master

6. 已经commit的内容，版本回退
git reset --hard HARD^  ^^ ^^^ [向前回退n个版本]
git log 查看commit id
git reflog 查看提交记录,详细记录每条记录的commitid和title
git reset --hard commitid 可以回退/前往指定的版

另外，在暂存区(stage)的内容(即已经add，或者motify，但为commit) 也可以通过reset来撤销stage，即把修改从暂存区撤回到工作区  git reset --hard <file> ...



注意：
git reset --hard会回退到你最近提交的版本上，如果你修改了工作区的内容只add了没有commit  那么使用了这个命令之后会回退到你最近一次commit的那个版本，你在工作区的修改就会丢失掉


7. 工作区 暂存区 本地分支 远程分支
工作区 -->git add file -->暂存区 -->git commit -m "xxx" -->本地分支-->git push origin master-->远程分支

8. Git跟踪并管理的是修改，而非文件


git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别

注意 git diff  vs git diff HEAD -- file 的区别

git diff 是比较工作区和暂存区的区别
git diff 是比较工作区和分支的区别



9. git checkout -- file     git reset HEAD file   git rest --hard HEAD^  撤销修改
```
对于文件 myfile.txt

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
```
10. 删除文件 以及 恢复删除的文件。 git rm file  ;


11. 添加远程仓库&关联远程仓库
git remote add  origin xxxx;
git push -u origin master;  -u的作用建立本地仓库和远程仓库的关联
git pull origin master;
git clone

12. git分支策略
[Git 使用规范流程](http://www.ruanyifeng.com/blog/2015/08/git-use-process.html)

13. stash 用于bug分支，当有临时事情需要处理，而手上需求分支尚未完成。可以通过stash，把当前修改的内容藏匿起来。在有bug的分支 checkout一个bug分支，处理完成之后，在回到需求分支，通过stash pop 把藏匿的内容恢复，继续工作
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

14. 在新分支开发功能完毕，如果功能需要，则merge到相应的分支，然后新分支可以通过 git branch -d new_branch 删除。
如果功能开发后，不需要了，即没有merge到其他分支，还用上面的删除命令就会报new_branch分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用命令git branch -D new_branch。即用大写的D强行删除


15. 多人协作
(1) 拉取分支：
git clone xxx;git自动把本地master和远程master分支对应起来，并且远程仓库的名字默认为origin。可以通过git remote 或者git remote -v 来查看。
(2) 推送分支：
git push origin xxx;把本地xxx分支推送到远程服务器。如果远程存在xxx分支，就是同步内容；如果远程没有xxx分支，就是把本地分支push到远程，并且建立关联。

(3) 更新本地分支
git pull 
(4) 删除远程分支
git push origin :分支名

git push origin :<remote-branch> 或者 git push origin --delete <remote-branch>
清除已被远程分支删除的本地残余 git remote prune origin

另外：
分支有四种类型
*. master分支 用于release发布版本
*. dev 用于团队协作
*. bug 用于修复bug
*. feature 用于开发新功能

16. tag
git tag <name> 用于创建一个标签，默认为HEAD，也可以指定commitid
git tag -a <tagname> -m "taginfo" ；可以指定标签信息
git tag 查看所有标签
git push origin <tagname>可以推送一个本地标签
git push origin --tags可以推送全部未推送过的本地标签
git tag -d <tagname>可以删除一个本地标签
git push origin :refs/tags/<tagname>可以删除一个远程标签

17. 配置alias，提高效率
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD'
git config --global alias.last 'log -1'
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
git config --global alias.ref 'remote prune origin'

git config --global alias.xxx xxxxxxx或者 git config --global alias.xxx 'xxxxxxxxxx'
18. rebase

rebase 是一个很清爽的命令
$ git rebase <[origin/]branch>
rebase 用于重整分支到 fast-forward 可能
进阶：git rebase -i 可以重排 commit 历史
缺点：
rebase 解决冲突较为繁琐
警告：
rebase 时请认准上游
只能用 rebase 重整本地提交


另外：git rebase 还可以把多次提交合并为一个

git rebase -i HEAD~[number_of_commits]

19. cherry-pick

如果不想一次合并整个分支，那么…
cherry-pick 可以选择合并部分 commit
$ git cherry-pick <commits>

提交错分支，可以此复制 commit
可用于打包时挑选 bug fix

一个事实：
两人各提交完全相同的修改，合并时不会冲突

20. revert回滚一个分支的合并

21.git reflog

```
你提交了一个你不想要提交的代码，最后你通过使用硬重置(hard reset)使其回到了之前的状态。稍后，你意识到，在这个过程中你丢失了一些其他的信息，并想要退回或是至少能看一眼。git reflog命令可以帮你做到这一点。

一个简单的git log命令，显示你最近的提交信息，以及上一次，再上一次的提交信息，以此类推。

而git reflog显示的是所有head移动的信息。记住，它是在本地的，而不是你仓库的一部分，不会包含在推送(push)和合并中(merge)
```
22. git cherry-pick
```
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

23. git gc

清理内存
24. git bisect

25. 冲突
git pull 的时候可能冲突
git 亦使用 <<<< ==== >>>> 格式的标示
图形化解决冲突可能更直观
解决冲突后，add、commit 提交
放弃解决冲突，可以 $ git merge --abort


26. merge
merge 进阶：
 --squash 可以把 commits 合而为一
 --no-ff 可以禁止 fast-forward 从而产生 merge info

git merge --squash的使用场景&使用方法
feature分支修改提交了多次即多个commit，dev分支 merge feature分支时候，如果feature分支的commit messages是有意义的就保留多次commit信息，否则就使用
--squash进行合并，然后添加新的message进行提交


git merge --no-ff的使用场景&使用方法
feature分支修改提交了多次，即多个commit，dev分支 merge feature分支时，fast-forward方式就是当条件允许的时候，git直接把HEAD指针指向合并分支的头，完成合并。属于“快进方式”，不过这种情况如果删除分支，则会丢失分支信息。因为在这个过程中没有创建commit

27. git show

git fsck --lost-found
这里你可以看到丢失的提交，你可以使用git show [commit_hash]来查看这些提交所包含的改动或者是使用git merge [commit_hash]来恢复它。
git fsck比reglog有一个优势。比如你删除了一个远端分支并且克隆了仓库，使用fsck命令你可以搜索并恢复该远端分支

28. git fsck
file system check
git fsck 

29. git tag

### tag是什么
tag就是标签，git也有在历史状态的关键点“贴标签”的功能--一般人们用这个功能来标记发布点

### 如何打tag
本地打标签

git tag v*** 2bcd97c

这个就是给2bcd97c的提交打一个标签，用于以后容易区分

本地删除标签

git tag -d v****

本地的标签如何PUSH到服务器端

将本地的标签push到服务器

git push origin v*****

从服务器端删除标签

git push origin :refs/tags/v****

#####如何使用tag
先 git clone 整个仓库，然后 git checkout tag_name 就可以取得 tag 对应的代码了。

但是这时候 git 可能会提示你当前处于一个 detached HEAD 状态，因为 tag 相当于是一个快照，是不能更改它的代码的，如果要在 tag 代码的基础上做修改，你需要一个分支：git checkout -b branch_name tag_name



.参考
[Git 使用规范流程](http://www.ruanyifeng.com/blog/2015/08/git-use-process.html)
[Git分支管理策略](http://www.ruanyifeng.com/blog/2012/07/git.html)
[Git flow工作流程](http://www.ruanyifeng.com/blog/2015/12/git-workflow.html)

[廖雪峰git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
[《Git Community Book 中文版》](http://gitbook.liuhui998.com/index.html)
[《Pro Git 中文版》]( http://git-scm.com/book/zh/)
[《Getting Started – Git-Flow》] (http://yakiloo.com/getting-started-git-flow/)
[让你的Git水平更上一层楼的10个小贴士](http://blog.jobbole.com/75348/)
