Git 工具代码

git remote add origin (网站)
git push origin main//推送
git pull//拉取
git clone
git add .
git commit -m "message"

 git config --global user.name
 git config --global user.email

git -a -m "message"一次完成添加和提交 == git -am "message"

创建分支: git branch 分支名

切换分支:git checkout 分支名 或者是 git switch 分支名

git status/git diff 查看冲突

git merge --abort 终止冲突修改

git reset --soft 版本号

git reset --hard 版本号

a = a + b

a是目前的分支

b 是merge后面的分支

git merge 分支名 //产生一次提交

删除分支 git branch -d 分支名

大写D是强制删除分支

rebase //在哪个分支执行就是变哪个分支的基,,多次提交

alias 别名="指令"    别名操作





2025-3-13

改变hosts后,使用原生git上传到github

```
git 无权限访问 The TLS connection was non-properly terminated
这样解决
git config --global  --unset https.https://github.com.proxy
git config --global  --unset http.https://github.com.proxy
然后
git config --global --unset http.proxy
git config --global --unset https.proxy
可以开始验证
username为你登录github的账号名(一般为邮箱地址)
注意! 密码为github上开发者选项中生成的令牌!
```

如何配置hosts达到不用VPN就能访问github

```
windows上有watt Toolkit加速器免费加速访问
https://github.com/521xueweihan/GitHub520?tab=readme-ov-file
该项目可以提供hosts访问github
下载Switchhost软件持续更新hosts
```

如果jetbrains上git工具无法push,需要给jetbrains生成一个令牌进行推送
