## git之分支合并的准则

新开了分支 branch 去完成某个 feature，完成之后要 merge 到 master 分支上，此时我们应该怎么操作了？

比如分支 branch 和 master，在合并之前，一般要将 master 分支当前最新的 commit 合并到 branch 上面，因为你的 branch 的起点此时可能已经不是 master 分支最新的 commit 了。切换到 branch 分支上运行 git merge master，当 git 提示 “Already up-to-date”，这说明当前所在的分支 branch 比 master 分支还要新，branch 上的 commit 都是 master 上面的最新的了，这个时候就不需要合并了，然后切回到 master 分支上面，将分支 branch 合并到 master 上面，整个流程就 ok 了。

