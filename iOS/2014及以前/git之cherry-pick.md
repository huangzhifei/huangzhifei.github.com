
## git之cherry-pick

cherry-pick 可以选择某一个分支中的一个或多个commit来进行操作。

比如：

我们有一个稳定的版本分支v2.0，另外正在开发的版本分支为v3.0 ，我们不能直接把两个分支合并，会导致稳定版本混乱，但是我们又想要增加v3.0里面一些功能到v2.0中，这里就要使用cherry-pick了，其实就是对已经存在 commit 进行在次提交。

```
git cherry-pick commit id commit id commit id
```
commit id 就是每条 commit 的 hash 值，这条命令可以同时 pick 出多条 commit，并列写出多条 commit id 就行了，commit id 我们可以只写前几位，当我们执行完 cherry-pick 后，将会生成一个新的提交，这个新的提交的 hash 值和原来的不同，但是标识名是一样的。


### 简单用法 

```
git checkout v2.0
git cherry-pick 38361a55 (这个是位于 v3.0 中的某条 commit）
```

```
$ git log
commit 38361a55138140827b31b72f8bbfd88b3705d77a
Author: huangzhifei <huangzhifei@jrq.com>
Date:   Mon Nov 20 11:16:52 2017 +0800
```

#### 1、如果顺利，就会正常提交
结果：

	Finished one cherry-pick.
	On branch v2.0分支
	Your branch is ahead of 'origin/old_cc' by 3 commits.

#### 2、如果在 cherry-pick 的过程中出现了冲突

	Automatic cherry-pick failed.
	After resolving the conflicts,mark the corrected paths with 'git add <paths>' or 'git rm 	<paths>'and commit the result with:
	git commit -c 15a2b6c61927e5aed6111de89ad9dafba939a90b
	或者:
	error: could not apply 0549563... dev
	hint: after resolving the conflicts, mark the corrected paths
	hint: with 'git add <paths>' or 'git rm <paths>'
	hint: and commit the result with 'git commit'
	
就根普通的冲突一样，手工解决一下。

#### 1、使用 git status 查看哪些文件出现冲突

```
You are currently cherry-picking commit 6b86c82.
  (fix conflicts and run "git cherry-pick --continue")
  (use "git cherry-pick --abort" to cancel the cherry-pick operation)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   UtilityKits/AppDelegate.m

no changes added to commit (use "git add" and/or "git commit -a")
```

#### 2、修改这个文件后保存（解决完冲突）

#### 3、添加修改的文件

```
git add UtilityKits/AppDelegate.m 或 git add .
```

#### 4、此时可以运行下面的命令，然后会出现 commit 编辑界面，继续使用之前的那个 commit

```
git cherry-pick --continue
```
如果此时依然有冲突，重复上面的就好了。

#### 或

#### 4.1 重新 commit 一次，然后提交

#### 中止

```
git cherry-pick --abort
```
如果不想合并了，就直接终止吧

总结：cherry-pick 这样的命令确实很好实用，好好利用。


参考文章：

[http://www.jianshu.com/p/08c3f1804b36]()


