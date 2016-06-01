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

另外：
分支有四种类型
*. master分支 用于release发布版本
*. dev 用于团队协作
*. bug 用于修复bug
*. feature 用于开发新功能



.参考
[Git 使用规范流程](http://www.ruanyifeng.com/blog/2015/08/git-use-process.html)
[廖雪峰git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
