
## Masonry小知识

### 介绍

Masonry 源码：https://github.com/Masonry/Masonry

Masonry是一个轻量级的布局框架 拥有自己的描述语法 采用更优雅的链式语法封装自动布局 简洁明了 并具有高可读性 而且同时支持 iOS 和 Max OS X。

我们先来看一段官方的sample code来认识一下Masonry

```oc
[view1 mas_makeConstraints:^(MASConstraintMaker *make) {
    make.edges.equalTo(superview).with.insets(padding);
}];
```

### Masonry支持的属性

``` oc
@property (nonatomic, strong, readonly) MASConstraint *left;
@property (nonatomic, strong, readonly) MASConstraint *top;
@property (nonatomic, strong, readonly) MASConstraint *right;
@property (nonatomic, strong, readonly) MASConstraint *bottom;
@property (nonatomic, strong, readonly) MASConstraint *leading;
@property (nonatomic, strong, readonly) MASConstraint *trailing;
@property (nonatomic, strong, readonly) MASConstraint *width;
@property (nonatomic, strong, readonly) MASConstraint *height;
@property (nonatomic, strong, readonly) MASConstraint *centerX;
@property (nonatomic, strong, readonly) MASConstraint *centerY;
@property (nonatomic, strong, readonly) MASConstraint *baseline;

```
这些属性与NSLayoutAttrubute的对照表如下

![](https://github.com/huangzhifei/huangzhifei.github.com/raw/master/images/masonry-1.jpg)


1. 很多时候分不清 centerX 与 centerY，这里特意说明一下，他与正常的autolayout是反的
centerX 对应的是横向中点，也就是设置了centerX 就不能在设置leading与trailing了
centerY 对应的是纵向中点，也就是设置了centerY 就不能在设置top与bottom了。

2. 其中leading与left 、trailing与right 在正常情况下是等价的，但是当一些布局是从右至左时(比如阿拉伯文) 则会对调，换句话说就是基本可以不理不用，用left和right就好了。

### Masonry用例分析

```oc
UIView *sv = [UIView new];
//在做autoLayout之前 一定要先将view添加到superview上 否则会报错
[self.view addSubview:sv];
//mas_makeConstraints就是Masonry的autolayout添加函数 将所需的约束添加到block中行了
[sv mas_makeConstraints:^(MASConstraintMaker *make) {
//将sv居中(很容易理解吧?)
    make.center.equalTo(ws.view);
     
    //将size设置成(300,300)
    make.size.mas_equalTo(CGSizeMake(300, 300));
}];
```
这里有两个问题要分解一下

1. 在 Masonry 中能够添加 autolayout 约束的有三个函数

``` oc
- (NSArray *)mas_makeConstraints:(void(^)(MASConstraintMaker *make))block;
- (NSArray *)mas_updateConstraints:(void(^)(MASConstraintMaker *make))block;
- (NSArray *)mas_remakeConstraints:(void(^)(MASConstraintMaker *make))block;
```
	说明：
	mas_makeConstraints 只负责新增约束 Autolayout不能同时存在两条针对于同一对象的约束 否则会报错 
	mas_updateConstraints 针对上面的情况会更新在block中出现的约束不会导致出现两个相同约束的情况，其实使用update是可以代替make的
	mas_remakeConstraints 则会清除之前的所有约束 仅保留最新的约束


2. MACRO 宏
	equalTo 和 mas_equalTo 的区别在哪里呢? 其实 mas_equalTo 是一个 MACRO,他支持的类型除了了NSNumber之外，还支持CGPoint、CGSize、UIEdgeInsets。
	
	
### 布局UIScrollView

参照文章[http://www.pixeldock.com/blog/uiscrollview-and-auto-layout/]()

我们都知道UIScrollView是有点不一样的，他要先知道contentSize，但是了contentSize 又要依赖于他本身，看下面的代码我们如何实现这样一个自动布局
	
```oc
	UIScrollView *scrollView = [[UIScrollView alloc] init];
	scrollView.backgroundColor = [UIColor whiteColor];
	[sv addSubview:scrollView];
	[scrollView mas_makeConstraints:^(MASConstraintMaker *make) {
   	 	make.edges.equalTo(sv).with.insets(UIEdgeInsetsMake(5,5,5,5));
	}];
	
	// 小技巧：给所有的subView加一个container，然后设置container的约束与scrollView的关系
	UIView *containter = [UIView new];
	[scrollView addSubview:container];
	[container mas_makeConstraints:^(MASConstraintMaker *make) {
    		make.edges.equalTo(scrollView);
    		// 如果我们需要竖向的滑动 就把width设为和scrollview相同
			// 如果需要横向的滑动 就把height设为和scrollview相同
    		make.width.equalTo(scrollView);
	}];
	
	int count = 10;
	UIView *lastView = nil;
	for ( int i = 1 ; i <= count ; ++i ) {
	    UIView *subv = [UIView new];
	    [container addSubview:subv];
	    subv.backgroundColor = [UIColor colorWithHue:( arc4random() % 256 / 256.0 )
	                                      saturation:( arc4random() % 128 / 256.0 ) + 0.5
	                                      brightness:( arc4random() % 128 / 256.0 ) + 0.5
	                                           alpha:1];
	     
	    [subv mas_makeConstraints:^(MASConstraintMaker *make) {
	        make.left.and.right.equalTo(container);
	        make.height.mas_equalTo(@(20*i));
	         
	        if ( lastView ) {
	            make.top.mas_equalTo(lastView.mas_bottom);
	        }
	        else {
	            make.top.mas_equalTo(container.mas_top);
	        }
	    }];
	     
	    lastView = subv;
	}
	[container mas_makeConstraints:^(MASConstraintMaker *make) {
	    make.bottom.equalTo(lastView.mas_bottom);
	}];
```

这里的关键就在于container这个view起到了一个中间层的作用 能够自动的计算uiscrollView的contentSize

### 小结
	
	Masonry 是一个非常优秀的autolayout的库，他里面还有很多高级的玩法，大家有兴趣的可以去看他的demo。
	
