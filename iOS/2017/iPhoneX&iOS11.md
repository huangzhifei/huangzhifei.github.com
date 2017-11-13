###iPhone X & iOS 11 适配

1、缺少iPhone X的启动图

   在 xcassets 中添加LaunchImage，他里面就会出现 iPhoneX的启动图。
	
2、导航栏的自定义返回按钮样式不对(主要分两种）
	
   a、自定义backBarButtonItem
	
   backBarButtonItem 的图片会很怪异，这是在iOS 11时改变了里面的布局，使用下面的代码可以解决.
   
   ```oc
   
	@implementation UIViewController (CustomBackBarButtonItem)

- (void)customBackBarButtonItem {
    if (@available(iOS 11.0, *)) {
        UIImage *image = [[UIImage imageNamed:@"ic_return"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
        UIBarButtonItem *backButtonItem = [[UIBarButtonItem alloc] initWithTitle:@""
                                                                           style:UIBarButtonItemStylePlain
                                                                          target:nil
                                                                          action:nil];
        self.navigationItem.backBarButtonItem = backButtonItem;
        self.navigationController.navigationBar.backIndicatorImage = image;
        self.navigationController.navigationBar.backIndicatorTransitionMaskImage = image;
    } else {
        UIBarButtonItem *backButtonItem = [[UIBarButtonItem alloc] initWithTitle:@""
                                                                             style:UIBarButtonItemStylePlain
                                                                            target:nil
                                                                            action:nil];
        
        UIImage *image = [UIImage imageNamed:@"ic_return"];
        UIImage *highlightedImage = [UIImage imageNamed:@"ic_return"];
        [backButtonItem setBackButtonBackgroundImage:[image resizableImageWithCapInsets:UIEdgeInsetsMake(0, image.size.width, 0, 0)] forState:UIControlStateNormal barMetrics:UIBarMetricsDefault];
        [backButtonItem setBackButtonBackgroundImage:[highlightedImage resizableImageWithCapInsets:UIEdgeInsetsMake(0, image.size.width, 0, 0)] forState:UIControlStateHighlighted barMetrics:UIBarMetricsDefault];
        [backButtonItem setBackButtonTitlePositionAdjustment:UIOffsetMake(0, -60) forBarMetrics:UIBarMetricsDefault];
        self.navigationItem.backBarButtonItem = backButtonItem;
    }
}
```

  b、自定义leftBarButtonItems
  
  在iOS 11下面我们怎么也无法正常的把返回的按钮尽可能的往左靠近，他就是那么坚挺的偏右，使用下面的方法解决.
  
  ```oc
  UIBarButtonItem *rightBarItem = [[UIBarButtonItem alloc] initWithImage:self.configuration.moreItemImage style:UIBarButtonItemStylePlain target:self action:@selector(onTappedMoreItem:)];
    self.navigationItem.rightBarButtonItem = rightBarItem;
    
    UIView *backView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 100, 44.0)];
    UIButton *backBtn = [[UIButton alloc] initWithFrame:CGRectMake(0, 0, 44.0, 44.0)];
    [backBtn setImage:self.configuration.backItemImage forState:UIControlStateNormal];
    [backBtn addTarget:self action:@selector(onTappedBackButton:) forControlEvents:UIControlEventTouchUpInside];
    [backView addSubview:backBtn];
    if (@available(iOS 11.0, *)) {
        backBtn.contentEdgeInsets = UIEdgeInsetsMake(0, -20, 0, 0);
        backBtn.imageEdgeInsets = UIEdgeInsetsMake(0, -20, 0, 0);
    }

    UIButton *closeBtn = [[UIButton alloc] initWithFrame:CGRectMake(40.0, 0, 44.0, 44.0)];
    closeBtn.hidden = YES;
    [closeBtn setImage:self.configuration.closeItemImage forState:UIControlStateNormal];
    closeBtn.titleLabel.textAlignment = NSTextAlignmentLeft;
    [closeBtn.titleLabel setFont:[UIFont systemFontOfSize:14.0]];
    [closeBtn setTitleColor:[UIColor whiteColor] forState:UIControlStateNormal];
    [closeBtn addTarget:self action:@selector(onTappedCloseButton:) forControlEvents:UIControlEventTouchUpInside];
    [backView addSubview:closeBtn];
    self.closebutton = closeBtn;
    if (@available(iOS 11.0, *)) {
        closeBtn.contentEdgeInsets = UIEdgeInsetsMake(0, -20, 0, 0);
        closeBtn.imageEdgeInsets = UIEdgeInsetsMake(0, -20, 0, 0);
    }

    UIBarButtonItem *leftBarItem = [[UIBarButtonItem alloc] initWithCustomView:backView];
    UIBarButtonItem *spaceButtonItem = [[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemFixedSpace target:nil action:nil];
    spaceButtonItem.width = -20.f;
    [self.navigationItem setLeftBarButtonItems:@[spaceButtonItem, leftBarItem]];
  ```
  
  主要是的起作用的代码是
  ```oc
  if (@available(iOS 11.0, *)) {
        backBtn.contentEdgeInsets = UIEdgeInsetsMake(0, -20, 0, 0);
        backBtn.imageEdgeInsets = UIEdgeInsetsMake(0, -20, 0, 0);
    }
  ```