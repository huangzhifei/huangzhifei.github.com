## Xcode上的代码格式调整工具

今天要说的主角就是 XcodeClangFormat（彻底退出Xcode）

[https://huangzhifei.github.com/resources/XcodeClangFormat.zip]()

安装上面的app，运行时选择一个自定义的 clangformat 文件

![](https://github.com/huangzhifei/huangzhifei.github.com/raw/master/images/xcodeClang.png)

clangformat文件下载

[https://huangzhifei.github.com/resources/clang-format.zip]()

在 macOS Sierra（10.12）以上，插件会自动被加载，我们要去  系统偏好--->扩展 里面检查这个插件是否正确加载

![](https://github.com/huangzhifei/huangzhifei.github.com/raw/master/images/xcodeClangSetting.png)

如果正常加载了，就在去给此插件设置一个快捷键，方便我们使用。依然是在 系统偏好--->键盘里面，选择 App 快捷键，然后点击加号去给Xcode添加 Format Source Code，定义好快捷健

![](https://github.com/huangzhifei/huangzhifei.github.com/raw/master/images/xcodeClangShortcut.png)

重新打开Xcode，在Editor菜单里面会出现一个 之前我们取的名字的插件（例如：Format Source Code）

![](https://github.com/huangzhifei/huangzhifei.github.com/raw/master/images/xcodeClangShow.png)

如果后面没有出现快捷键，我们就去Xcode的偏好里面去设置吧

***tip***

1、快捷键建议设置一个方便用的，这样 command + A 全选后，直接使用我们定义的快捷键就能把全部代码规范法。  

2、当然偶尔会有不灵的时候（Xcode还偶尔挂掉了），退出Xcode重新打开就好了。

3、工具只是一种辅助，大家还是要严格一下自己的编码习惯。

4、ClangFormat 文件大家可以打开看，会发现里面规则很简单，可以自行学习了解往里面添加常用的规范。
