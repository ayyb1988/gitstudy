1. git checkout --track origin/feature
error: there are still refs under 'refs/heads/feature'
fatal: Failed to lock ref for update: Is a directory


参考
[Accidently Pulled Down Branch with the Same Name](git checkout --track origin/feature
error: there are still refs under 'refs/heads/feature'
fatal: Failed to lock ref for update: Is a directory)


2. Why do I get error: RPC failed; result=52, HTTP code = 0 fatal: The remote end hung up unexpectedly when pushing to github?
参考[RPC failed](http://stackoverflow.com/questions/18436812/why-do-i-get-error-rpc-failed-result-52-http-code-0-fatal-the-remote-end-h)

3. 解决github push错误The requested URL returned error: 403 Forbidden while accessing
```
解决方案：

vim .git/config

修改

[plain] view plain copy print?在CODE上查看代码片派生到我的代码片
[remote "origin"]  
    url = https://github.com/wangz/example.git  
为：
[plain] view plain copy print?在CODE上查看代码片派生到我的代码片
[remote "origin"]  
    url = https://wangz@github.com/wangz/example.git  
再次git push，弹出框输入密码，即可提交
```

http://blog.csdn.net/happyteafriends/article/details/11554043
