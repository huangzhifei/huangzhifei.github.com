## git之无法pull仓库

git 无法 pull 仓库，报错 refusing to merge unrelated histories

场景：

在 github 新建一个仓库，写了 license，然后把本地一个写了很久的仓库上传，先 pull，由于两个仓库不同，发现 refusing to merge unrelated histores，无法 pull，因为他们两其实是不同的项目，要把这两个不同的项目合并，git 需要添加一句代码，如下：

```
git pull origin master --allow-unrelated-histories
```

这样就把可以了，然后就可以正常 push & pull 了。

