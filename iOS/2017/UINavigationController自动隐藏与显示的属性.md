## UINavigationController自动隐藏与显示的属性

有很多应用中，会发现在在滚动试图，点击试图，滑动试图的时候，导航栏自动隐藏和显示，当时我的想法就是在触发事件中做操作（手动隐藏和显示导航栏），后来发现这个功能系统其实就可以帮忙实现，下面就简单介绍一下。

UINavigationController中的几个属性

####1、hidesBarsWhenKeyboardAppears 属性

当键盘弹出的时候，导航栏自动隐藏，默认NO，注意：如果只设置这个属性为YES，键盘出现的时候，导航栏就自动隐藏了，但是之后无论怎么操作，导航栏都不会再显示出来，所以需要配合 hidesBarsOnSwipe 或者 hidesBarsOnTap 使用，这样的话，导航栏就能自如的隐藏和展示了。

####2、hidesBarsOnTap 属性

self.navigationController.hidesBarsOnSwipe = YES;// 点击控制器的时候，导航栏自动隐藏和显示

####3、hidesBarsOnSwipe 属性

self.navigationController.hidesBarsOnSwipe = YES;// 上下滑动的时候，导航栏自动隐藏和显示

####4、interactivePopGestureRecognizer 属性

这个属性是只读的，用来操作控制器的手势返回滑动。

####5、hidesBottomBarWhenPushed 属性

需要注意的这个属性是在目标控制器中设置的，比如目标控制器为 targetVC

targetVC.hidesBottomBarWhenPushed = YES;

隐藏之后不会自动显示出来，还需手动设置，所以才要给目标控制器设置。
