参照文章 [http://www.jianshu.com/p/07c430fbde9e]()


UIStackView是iOS9之后推出的，主要包括了四大属性：axis、alignment、distribution、spacing。

### axis(轴)

主要设置UIStackView布局的方向：水平方向或垂直方向

```
typedef NS_ENUM(NSInteger, UILayoutConstraintAxis) {

UILayoutConstraintAxisHorizontal = 0,//水平

UILayoutConstraintAxisVertical = 1//垂直

};

```


### alignment

主要设置非轴方向子视图的对齐方式

```
typedef NS_ENUM(NSInteger, UIStackViewAlignment) {

	UIStackViewAlignmentFill,//子视图填充StackView

	UIStackViewAlignmentLeading,//子视图左对齐（axis为垂直方向而言）

	UIStackViewAlignmentTop = UIStackViewAlignmentLeading,//子视图顶部对齐（axis为水平方向而言）

	UIStackViewAlignmentFirstBaseline, // 按照第一个子视图的文字的第一行对齐，同时保证高度最大的子视图底部对齐（只在axis为水平方向有效）

	UIStackViewAlignmentCenter,//子视图居中对齐

	UIStackViewAlignmentTrailing,//子视图右对齐(axis为垂直方向而言）

	UIStackViewAlignmentBottom = UIStackViewAlignmentTrailing,//子视图底部对齐（axis为水平方向而言）

	UIStackViewAlignmentLastBaseline, // 按照最后一个子视图的文字的最后一行对齐，同时保证高度最大的子视图顶部对齐（只在axis为水平方向有效）

} NS_ENUM_AVAILABLE_IOS(9_0);

```

### distribution

设置轴方向上子视图的分布比例（如果axis是水平方向，也即设置子视图的宽度，如果axis是垂直方向，则是设置子视图的高度）

```
typedef NS_ENUM(NSInteger, UIStackViewDistribution) {

	UIStackViewDistributionFill = 0,

	UIStackViewDistributionFillEqually,

	UIStackViewDistributionFillProportionally,

	UIStackViewDistributionEqualSpacing,

	UIStackViewDistributionEqualCentering,

} NS_ENUM_AVAILABLE_IOS(9_0);

```

例子见UtilityKit/StackView

