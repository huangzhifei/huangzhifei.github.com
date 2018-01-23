## MGSwipeTableCell实现原理简单分析

先上链接地址 [MGSwipeTableCell](https://github.com/MortimerGoro/MGSwipeTableCell)

感动的是至今作者还在维护这个项目，iOS 5出来的，虽然 iOS 8出了新的API，可以增加自定义右边滑出的菜单，但是他依然不支持左边出菜单以及 iOS7。（理由够强烈了吧）

这篇文章不讲使用方法，作者在 README 里面说的很详细。初读了代码，想看看是怎么个实现思路（好奇宝宝），发现这个库写的很好，很强大（虽然代码格式不怎么规范，虽然有一个地方在方法名后面直接多打了分号，但是不影响，not care）


## 实现思路

### 1、继承自 UITableViewCell

完美集成自 UITableViewCell，给其增加一些属性、代理方法、动画方式等等，方法用户自定义（比如支持多行滑动）

### 2、滑出菜单

左右滑出菜单，都是使用继承自 UIButton，事件方便，图文并茂，不用担心图文太多布局

### 3、手势

MGSwipeTableCell 内部滑动手势并没有使用 UISwipeGestureRecognizer，而是使用 UIPanGestureRecognizer.


### 4、结束菜单

当我们在单滑模式下滑出菜单后，点击内容视图（tableView）任何区域都会完美收起菜单，而不会被 UITableViewCell 上的子控件拦截事件，其实是他偷偷的盖了一层和 tableView 的 bounds 一样大小的背景透明视图，增加 tap 手势去收起菜单。

但是问题来了：
菜单出现后，我们如果点击菜单上面的 button 是要有响应的，所以上面说到的蒙层 View，我们需要继承 UIView，然后复写 hit 去处理当前点击区域在哪。（具体看源码，还涉及到穿透）

### 5、滑出菜单原理

滑出时，他做了下面几点动作

##### a、创建 cell 的截图
这里有个小优化，截图里面并不包含分割线（分割线根着滑很奇怪），怎么才能完美截出不带分割线的图了，我们可以使用 cell 的宽，但是使用 cell.contentView.height。

##### b、隐藏 cell 上面 contentView 下的所有子 view

##### c、添加刚才的截图

##### d、动画
这里的动画，假设我们只取最简单的从左到右，从右到左，作者使用了 CADisplayLink 去算移动的偏移量，然后使用 CGAffineTransformMakeTranslation(tx, ty) 去做移动动画，**这里是不是也给了大家思路去实现自己的动画**，基本上自定义动画都是需要使用 CADisplayLink 在配置系统提供的 C 的仿射变化接口去实现。

### 6、收起菜单后，显示 contentView 下的所有子 view



### 总结

源码里面有很多细节，很值得仔细分析与学习。

### 自己如何简单实现

[看后面部分内容](http://www.aizhuanji.com/a/7w2dJdJW.html)

一句话总结就是在你的 view 下面放一个含有左右要出现的菜单，然后上面放你实际的内容盖住，增加手势，左右滑动调整约束来展示。

