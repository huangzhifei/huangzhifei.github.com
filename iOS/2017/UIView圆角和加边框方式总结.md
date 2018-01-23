## UIView圆角和加边框方式总结

### 最简单的

直接设置 view 自带的 layer 属性

	view.layer.cornerRadius = 5.0f;

如果有子 view， 那还要加上
	
	view.clipsToBounds = YES;
	
如果需要边框，也简单，加上 layer 的边框设置就可以

	view.layer.borderWidth = 10.0f;
	view.layer.borderColor = [UIColor yellowColor].CGColor;
	
优点就是简单，缺点是有严重的黑边和锯齿，视图越小越明显，因为使用了 clipsToBounds 会触发离屏渲染。



### mask layer

使用贝塞尔曲线，并根据其路径，得到一个遮罩 layer，将其设置 layer 的 mask，盖掉其他部分，剩余中间的部分，可以出现各种你想要的形状。

#### 1、赋值给 mask

	UIBezierPath* maskPath = [UIBezierPath bezierPathWithRoundedRect:self.bounds byRoundingCorners:corners cornerRadii:size];
    
    CAShapeLayer* maskLayer = [CAShapeLayer layer];
    maskLayer.frame = self.bounds;
    maskLayer.path = maskPath.CGPath;
    
    self.layer.mask = maskLayer;

#### 2、添加到 layer 上

	UIBezierPath* maskPath = [UIBezierPath bezierPathWithRoundedRect:v.bounds byRoundingCorners:UIRectCornerAllCorners cornerRadii:CGSizeMake(halfW, halfW)];

	CAShapeLayer* maskLayer = [CAShapeLayer layer];
	maskLayer.frame = view.bounds;
	maskLayer.fillColor = [UIColor whiteColor].CGColor;
	maskLayer.path = maskPath.CGPath;
	maskLayer.backgroundColor = [UIColor greenColor].CGColor;

	[view.layer addSublayer:maskLayer];
	
如果是添加到 layer 上面，那还需要设置 clipsToBounds 去裁剪圆角，又会产生离屏渲染的。

#### 3、使用 UIView+UtCornerExtension.h

