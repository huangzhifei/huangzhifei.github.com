## UITableViewCell左滑与右滑菜单

UITableViewCell的侧滑可以很方便的进行一些操作，比如删除、标记等等，有一个很好用的第三方库[MGSwipeTableCell](https://github.com/MortimerGoro/MGSwipeTableCell)，可以帮助我们快速实现Cell的左滑和右滑，这个库有详细的使用说明，这里就不多说了。但是有些时候，我们不想只为了某一个界面就导入这么一个库，我们也可以使用系统自带实现一些简单的侧滑。


### 实现

#### iOS8之前

在iOS8以前我们可以通过实现 UITableView 下面的代理方法就可以实现一个简单的侧滑，但是这种只能有一个按钮，比较局限。

```
- (BOOL)tableView:(UITableView *)tableView canEditRowAtIndexPath:(NSIndexPath *)indexPath
{
    return YES;
}

- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath
{
    
}

- (BOOL)tableView:(UITableView *)tableView canMoveRowAtIndexPath:(NSIndexPath *)indexPath
{
    return YES;
}

- (NSString *)tableView:(UITableView *)tableView titleForDeleteConfirmationButtonForRowAtIndexPath:(NSIndexPath *)indexPath 
{
    return @"删除";
}
```

#### iOS8及之后

系统提供了UITableViewRowAction，只需要实现下面这个代理方法就可以了，返回的是UITableViewRowAction数组，可以实现多个按钮。

```
- (nullable NSArray<UITableViewRowAction *> *)tableView:(UITableView *)tableView editActionsForRowAtIndexPath:(NSIndexPath *)indexPath
{
    UITableViewRowAction *cancle = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleDefault title:@"取消拉黑" handler:^(UITableViewRowAction * _Nonnull action, NSIndexPath * _Nonnull indexPath) {
        
		// do something ...
    }];
    cancle.backgroundColor = RGBCOLORV(0xdedede);
    return @[cancle];
}
```

注意这里有一个坑！在iOS8上面，只实现这个方法并不能侧滑，还需要加上下面这个方法，什么都不用实现就可以，这估 这是系统的bug。

```
/// fixbug: 在iOS8.3真机上面，不重写这个方法，就无法左滑 
- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath
{
}
```

如果我们想要修改UITableViewRowAction的字体颜色，可以重写 cell 的 layoutsubviews 方法，遍历后去改变颜色

```
for (UIView *sbv in self.subviews) {
        for (UIView *sbv2 in sbv.subviews) {
            NSString *class_str = NSStringFromClass([sbv2 class]);
            //NSLog(@"class_str = %@", class_str);
            if ([class_str rangeOfString:@"UITableViewCellActionButton"].location != NSNotFound) {
                for (UIView *v in sbv2.subviews) {
                    NSString *class_str2 = NSStringFromClass([v class]);
                    //NSLog(@"class_str2 = %@", class_str2);
                    if ([class_str2 rangeOfString:@"UIButtonLabel"].location != NSNotFound) {
                        UILabel *l = (UILabel *)v;
                        //NSLog(@"label = %@", l);
                        l.textColor = ; //你想要更改的颜色
                        l.font = [UIFont systemFontOfSize:16]; //你想要的字体
                    }
                }
            }
        }
    }
```


**上面说的系统提供只能左滑动，右边出现菜单，如果想右滑左边出菜单，或者左右都想要菜单怎么办？**

***tip：UITableView 的编辑模式和这个还有点不一样。***

这可以参照文章最开始出现那个库。

