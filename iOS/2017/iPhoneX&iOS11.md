## iPhone X 和 iOS 11 适配

#### 1、缺少iPhone X的启动图

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
 
#### 3、tableView

   磨人的小妖精呀，基本上项目中遍地都是tableview，但是这次她出了问题！
   
   a、groupe 样式下的 setion header \ footer 高度很大，解决办法

      _tableView.estimatedRowHeight = 0;
      _tableView.estimatedSectionHeaderHeight = 0;
      _tableView.estimatedSectionFooterHeight = 0;
   
   b、push 动画进来的时候，会有明显的从下往上偏移的动画，解决办法
   
   ```oc
   if (@available(iOS 11.0, *)) {
      _tableView.contentInsetAdjustmentBehavior = UIScrollViewContentInsetAdjustmentNever;
   }
   ```
   
   c、有时候 UITableView 与他里面的容器view (UITableViewWrapperView） 大小不一致，不管你怎么调整tableveiew的
   布局，死活是莫名其妙的有 20 的差距，使用下面的代码解决(这个不是iOS 11的适配，反而ios 11 下面没有)
   
   ```oc
   if ([self respondsToSelector:@selector(automaticallyAdjustsScrollViewInsets)]) {
       self.automaticallyAdjustsScrollViewInsets = NO;
   }
   ```
   
#### 4、搜索框调整
   iOS 11 使用新方法将搜索框附着到导航栏上。它需要把UISearchController赋值给navigationItem，看下面的代码
   
   ```oc
   self.searchController = [[UISearchController alloc] initWithSearchResultsController:nil];
   self.searchController.searchResultsUpdater = self;
   self.searchController.delegate = self;
   self.searchController.dimsBackgroundDuringPresentation = NO;
   self.searchController.hidesNavigationBarDuringPresentation = NO;

   self.searchController.searchBar.barTintColor = [UIColor colorWithRGB:0xf0f0f0];
   self.searchController.searchBar.placeholder = @"搜索";

   // 重要：参考 http://www.cocoachina.com/ios/20160805/17298.html
   self.definesPresentationContext = YES;
   if (@available(iOS 11.0, *)) {
      self.navigationItem.searchController = self.searchController;
   } else {
      self.tableView.tableHeaderView = self.searchController.searchBar;
   }
```
   
   
#### 5、导航栏的高度
   
   对于需要计算顶部高度的地方，应该动态获取状态栏和导航栏高度：
   
   ```oc
   
   // 动态获取状态栏高度 
   CGRect statusRect = [[UIApplication sharedApplication] statusBarFrame]; 
   // 动态获取标题栏高度
   CGRect navRect = self.navigationController.navigationBar.frame; 
   CGFloat topHeight = statusRect.size.height + navRect.size.height;
   
   ```
   
   对于需要计算 iPhone X 底部 homeIndicator 高度的地方，可以使用下面的代码：
   
   ```oc
   
   // 方法一：
   CGFloat bottomHeight = 0;
   if (@available(iOS 11.0, *)) {
       bottomHeight = [#ViewController#.view safeAreaInsets].bottom;
   }

   // 或方法二：
   #define IS_iPhoneX ([UIScreen mainScreen].bounds.size.height == 812.0)
   if (IS_iPhoneX) {
       bottomHeight = 34.f;
   }
   
   ```
   
   对于使用 Masonry 写 autoLayout 的，可以像下面这样：
   
   ```oc
   [self.webView mas_makeConstraints:^(MASConstraintMaker *make) {
      make.leading.mas_equalTo(0);
      make.trailing.mas_equalTo(0);
      make.top.mas_equalTo(0);

      if (@available(iOS 11.0, *)) {
          make.bottom.mas_equalTo(weakSelf.view.mas_safeAreaLayoutGuideBottom);
      } else {
          make.bottom.mas_equalTo(0);
      }
    }];
   
   ```
   
   
#### 总结
   目前基本上主要也就是这些地方需要适配，对于那些支持横屏的应用，那就要多少考虑一下安全区域了，如果只是竖屏应用，其实只要解决好
   tableview的安全适配基本上就没有什么问题了。
    
