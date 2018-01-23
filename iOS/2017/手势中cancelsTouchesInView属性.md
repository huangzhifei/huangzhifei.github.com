## 手势中cancelsTouchesInView属性

UIGestureRecognizer 中一个属性，默认值为YES，使用场景：

假设我们有一个 tableView，想增加一个手势来使用键盘消失

```
UITapGestureRecognizer * tapGesture = [[UITapGestureRecognizeralloc]initWithTarget:selfaction:@selector(dismissKeyBoard)];
[self.tableView addGestureRecognizer:tapGesture];
```

隐藏键盘

```
-(void)dismissKeyBoard {
  [_textField resignFirstResponder];
}
```

很正常的一个场景，那么问题来了，我们自定义了 UITableViewCell，里面怎么都会有一些按钮之类，如果我们点击到了这些控件上，键盘并不会退出，如果这个时候我们增加如下代码

```
tapGesture.cancelsTouchesInView = NO;
```

就 ok 了，看了 API 介绍总结一句话：cancelsTouchesInView 为 NO 时，在 parentView 加上的手势就能不冲突的跟原本 subview 中的元件 tap 事件同时被触发。



