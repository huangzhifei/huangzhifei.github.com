## git之提交一些小心得

大家在用git进行开发的时候，这里推荐一个可视化工作 SouceTree，但是使用工具会有一些小细节做不好，比如更新后在提交，会多出一条无用的合并 commit 记录，这条记录其实是不方便其他人查看 commit 日志的，下面简单介绍一个自己平时使用的流程，这里是使用 branch， 大家也可以使用 stash ，看个人爱好

1、拉取分支，比如从 master 上拉取，我们命令为 tmp_dev 分支，本地的分支；

2、在 tmp_dev 分支上开发完成后正常 commit；

3、切换回 master 分支更新代码，正常操作；

4、切换回 tmp_dev 分支，然后命令行输入 git rebase master，这个过程就如果出现冲突就解决冲突，解决完冲突后在进行 git rebase --continue，反复如此直到一切正常；

5、切换回 master 上进行 tmp_dev 合并到 master 上面，如果一切正常就可以直接推到远端了。

ps：

给 git 设置用户名称，因为设置完成之后，commit 上面就能正确的看到你的信息。

```oc
 　git config --global user.name "eric"
 　git config --global user.email huangzhifei2009@126.com 
```
注意一点，如果是传递的 --global 选项，那就只要这样设置一次就好了，git 总是会使用这个信息来处理你的操作，如果你希望在一个特定的项目中使用不同的名称或email，可以在在该项目中运行不带 --global 选项的命令。

