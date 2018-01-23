## edgesForExtendedLayout属性

### 方法一
在iOS 7后新增了下面这两个属性

```oc

self.edgesForExtendedLayout=UIRectEdgeNone;
self.automaticallyAdjustsScrollViewInsets=NO;

```
只要设置了 self.edgesForExtendedLayout = UIRectEdgeAll（默认）后就会让布局从状态栏开始，而不是从导航栏下面开始。

所以要使用上面的代码来杜绝这种情况。

### 方法二
**关闭导航栏半透明**

在iOS 6及之前，translucent默认都是NO，但是在iOS 7开始就默认都是YES了，所以我们要关闭掉，特别是我们给导航栏设置统一的颜色时，如果不关闭半透明，颜色值就永远设置不对，并且会影响两个页面push的动画。

```oc

self.navigationController.navigationBar.translucent = NO;

```
将半透明度关闭后，布局也将会从导栏下面开始了。

### 方法三

将 view 的布局直接相关顶部设置64（出了iphone X后就不能这样写死，因为他是88），此方法不如上面两种方式简洁，所以不推荐。
