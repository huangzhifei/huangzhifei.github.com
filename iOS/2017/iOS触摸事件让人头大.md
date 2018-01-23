## iOS触摸事件让人头大

瞎总结：

1、当触摸事件开始后，系统会先尝试分发事件，对整个 keyWindow 递归调用 hitTest

2、系统判别返回的 UIView 是不是 UIControl 的子类，如果是的话，直接将整个触摸事件交付给 UIControl 实例完成（比如：UIButton、UISwitch）

3、如果返回的是普通 UIView， 且此时有相应的 UIGestureRecognizer，将事件交由该 gesture 去处理，并同步走 touchesBegan 以及 touchesMove 流程，最终由相应的 gesture 决定调用 touchesEnded 还是 touchesCancelled （手势有属性就是用来控制这个的）

4、编不下去了。