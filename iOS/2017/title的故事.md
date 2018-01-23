## title的故事

###tabbarItem title 与 uiviewcontroller title

1. UIViewController 的 title
	self.title = @"xxxx" 使用这种方式来设置title会出现一个坑，
	他会同时对tabbar title也起作用
2. navigationItem 的 title
	self.navigationItem.title = @"xxxx" 使用这种方式just设置当前控制器的title，他不会影响到tabbar 
3. tabbarItem 的title
	self.tabbarItem.title = @"xxxx" 使用这种方式也只会设置其title的值，但是坑爹的地方是如果开发者使用了第一种方式，那么他就会影响到tabbarItem 的title的值。
	
	上面小细节，大家还是注意一下。
