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





.参考
[Git 使用规范流程](http://www.ruanyifeng.com/blog/2015/08/git-use-process.html)
[廖雪峰git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
