## github之fork的同步

### Configuring a remote for a fork

1、给 fork 配置一个 remote
2、使用 git remote -v 查看远程状态
	
	git remote -v
	origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
	origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)

3、添加一个将被同步给 fork 远程的上游仓库
	
	git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
	
4、再次查看状态确认是否配置成功。

	git remote -v
	origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
	origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
	upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
	upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
	
### Syncing a fork

1、从上游仓库 fetch 分支和提交点，传送到本地，并会被存储在一个本地分支 upstream/master 
	
	git fetch upstream
	remote: Counting objects: 75, done.
	remote: Compressing objects: 100% (53/53), done.
	remote: Total 62 (delta 27), reused 44 (delta 9)
	Unpacking objects: 100% (62/62), done.
	From https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY
	* [new branch]      master     -> upstream/master

2、切换到本地主分支(如果不在的话) 
	
	git checkout master
	
3、把 upstream/master 分支合并到本地 master 上，这样就完成了同步，并且不会丢掉本地修改的内容

	git merge upstream/master
	
4、如果想更新到 GitHub 的 fork 上，直接 git push origin master 就好了

