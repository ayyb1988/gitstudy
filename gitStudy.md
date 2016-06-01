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


5.参考
[Git 使用规范流程](http://www.ruanyifeng.com/blog/2015/08/git-use-process.html)
