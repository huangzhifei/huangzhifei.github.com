## StatusBar的故事

### 隐藏与显示

在 iOS7.0 以前我们都知道设置 status bar 显示与隐藏使用下面的代码

```
[UIApplication sharedApplication].statusBarHidden = YES;
或者通过 
[[UIApplication sharedApplication] setStatusBarHidden:YES withAnimation:YES]
```
iOS7.0 之后，这两个全局的方法不好用，本身这种全局方法在使用的时候就比较坑，因为可能只是某几个页面需要更改样式，改完后在页面切换的时候还要还原，但是如果使用下面提供的新的方法，就只会在当前页面有效果，系统会帮我们还原，并且效果会比我们自己控制的还原要好

```
- (BOOL)prefersStatusBarHidden {
    return YES;
}
```
每个 UIViewController 里面都有这个方法，我们只要在想要更换 status bar 显示与隐藏的页面重写这个方法就好了。

假设我们的应用中所有的界面都需要隐藏状态栏怎么办？可以在工程中的Info.plist文件中进行设置:

```
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>

```

### 样式

道理和上面的基本一样，使用新的实现方法

```
- (UIStatusBarStyle)preferredStatusBarStyle {
    return UIStatusBarStyleLightContent;
}
```

如果是全局，也是就在 info.plist 里面添加

```
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleDefault</string>
```
值可以选择。

