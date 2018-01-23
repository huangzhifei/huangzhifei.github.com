## 创建自定义bundle

bundle 文件在运行的时候不会被编译，此文件最好是存放一些静态资源文件。

#### 创建 bundle 文件

##### 1、右键 -> New File -> iOS -> Resource -> Settings Bundle
见下图：

![](https://huangzhifei.github.com/images/bundle.png)

##### 2、随便起个名字就行了，不需要去带.bundle后缀

##### 3、里面会自动生成一些文件，不需要的直接删掉，添加自己的文件或文件夹


#### 使用自定义的 bundle 文件

##### 1、错误的使用方法
我们总是习惯性的 Command + 单击，看里面有哪些方法，会发现下面这个方法

```
+ (nullable NSBundle *)bundleWithIdentifier:(NSString *)identifier;
```

自认为是 .bundle 的名字默认当做 Identifier，于是就想用这个方法获取自定义的 NSBundle 文件，代码:

```
//获取路径
NSString * path = [[NSBundle bundleWithIdentifier:@"YPhotoBundle"] pathForResource:@"Image" ofType:nil];

//拼接路径
path = [path stringByAppendingPathComponent:@"未选中.png"];

//获取图片对象
return [UIImage imageWithContentsOfFile:path];
```

结果发现，返回的bundle，path，image全部为nil，于是又仔细读一下它的官方文档:

```
//返回之前创建过并且拥有bundle identifier的NSBundle实例
Returns the previously created NSBundle instance that has the specified bundle identifier.

/**
   *如果找不到，则返回nil
   *好吧我承认结果就是找不到0.0，我也不知道如何在创建.bundle的时候给予他bundle identifier
   *因为在Methods for creating的方法里没有找到，如果有知道的大神，麻烦告知一下 Mark
**/
The previously created NSBundle instance that has the bundle identifier identifier. Returns nil if the requested bundle is not found.
```


##### 2、正确的使用方法

a、获取自定义的 .bundle 文件的路径

```
//自定义的.bundle文件也存在与应用的主目录下，所以还是需要从主目录来拼接路径
- (NSString *)bundlePath {
    //获取路径
    NSString * path = [[NSBundle mainBundle] pathForResource:@"YPhotoBundle" ofType:@"bundle"];

    return [path stringByAppendingPathComponent:@"Image"];
}
```

b、在引用图片的时候在使用方法就可以获取到YPhotoBundle.bundle里面Image下的资源文件

```
//获得正常未选中的图片
- (UIImage *)normalImage {
    //最后加不加.png都是可以的，因为没有重名的问题
    return [UIImage imageWithContentsOfFile:[self.bundlePath stringByAppendingPathComponent:@"未选中.png"]];
}
```

