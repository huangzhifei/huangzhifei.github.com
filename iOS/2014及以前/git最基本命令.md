### **会持续更新......**

使用 **git --help** 会显示出所有的命令，这篇文章存在的目的是为了更好的说明其中一些常用的命令。

其实最好的方法是自己在 github 上面创建一个项目，把命令过一遍，其实在你使用 git 命令的时候，控制台都有相应的提示。


## 状态相关命令

### 1、重置一些文件的修改
这个可能要分点情况(仔细看命令行就有提示)

**a、此文件已经在版本控制中，但是还没有被add(也就是不是新加的文件)**

在使用 git status 查看的时候，就会提示你这些命令，使用下面的命令就行

```
git checkout -- file
```
这样就能重置之前的修改
   
**b、此文件已经在版本控制中，并且已经 add 了**

在使用 git status 查看的时候，就会提示你下面的命令

```
git reset HEAD file
```
这样就能把刚才 add 的此文件移出来，然后在使用 a 的命令
   
### 2、修改本地最后一次的commit

有时候我们提交完了才发现漏掉了几个文件没有加，或者说提交信息写错了，又或者在次修改了某一个很小的地方，想要撤消刚才的commit，可以使用
--amend 选项来重新提交

```
git commit --amend
```   
此命令将使用当前的暂存区域快照提交，如果刚才提交完没有作任何改动，直接运行此命令的话，相当于重新编辑提交说明，但将要提交的文件快照和之前的一样。
比如我之前有一次commit了，我修改了 xxx 文件

```
git add .
git commit --amend
```
会弹出上次 commit 的信息，可以编辑，如果不想编辑，直接退出就好了

### 3、丢弃本地所有改动

```
git reset --hard 
或
git reset --hard origin/master
```

### 4、查看状态

```
git status
```
用来查看本地所有状态，比如：哪些文件修改、哪些文件已经被add了等等

### 5、查看日志

```
git log
```
比如查看本地的commit信息等等

### 6、回滚


## 提交与更新相关

### 1、获取远端最新内容

```
git pull 或 git pull origin master 或 git fetch
```

### 2、推送内容到远端

```
git push 或 git push origin master
```

### 3、提交

```
git commit -m 'message'
```
提交这一次的修改 commit，-m 后面是此次 commit 的描述信息

### 4、添加

```
git add . 或 git add xxx
```
前面的命令是一次添加完所有的修改，后面的命令是可以指定添加哪一个修改的文件


## tag 相关

### 1、添加 tag

```
git tag 或 git tag -l xx*
```
如果有 tag， 就会列出所有的 tag 或者列出匹配 xx 的tag

```
git tag xxx
```
会在本地打一个tag，名字叫 xxx，不过这样的 tag 没有附带信息，可能不方便后面查看

```
git tag -a xxx -m 'first version'
```
这样打出的 tag， -m 后面带的就是注释信息，方便日后查看（***目前还没有找到哪里会显示这个附加信息）***

### 2、删除 tag
很简单，只要知道 tag 的名字就行了

```
git tag -d xxx
```
这样就能删掉本地这个名字的 tag 了

如果我们想删除远端的 tag，我们可能需要执行多个命令

```
git tag -d xxx
git push origin :refs/tags/xxx
```
第一条命令是删掉本地的tag，然后第二命令删掉远端的tag

### 3、推送 tag 到远端
前面的操作都是在本的做的，最后我们都要把操作更新到远端的

```
git push --tag 或 
git push --tags 或 
git push origin --tags
```
这是把本地所有的 tag 全部推送到远端

```
git push origin tagname
```
这样推送指定的 tagname 到远端

