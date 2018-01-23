## cocoapod 发布自己的仓库

网上有一堆这样的教程，下面我只是总结几个关键步骤

#### 1、注册trunk

a、安装cocoapod

```
sudo gem install cocoapods
```

b、发送注册请求，如果注册过的就忽略

```
pod trunk register email 'username' --verbose
```
之后会给对应邮箱发一份邮箱，去邮箱验证一下就行了（基本上会被当成垃圾邮件)

### 2、github仓库
a、上传代码 github ，创建对应版本的tag，然后创建podspec文件

```
pod spec create 名字
```

b、修改这个podspec文件，偷懒的话可以随便找一个别人的，拿过来更改

c、上传 podspec 文件到cocoapod的仓库下面
```
pod trunk push Project_name.podspec --allow-warnings --verbose
```
这一步很关键，他会检查你之前生成的podspec文件有没有出错，他会去对应的tag下面拉取代码编译，如果成功会返回很明显的成功信息

### 3、查找发布的pod库
当我们按照前面的方法正确操作后，很多时候我们使用search都会发现无法搜索到，这个时候我们执行下面的方法

a、执行 pod setup 命令
如果执行上面命令，在进行 pod search ‘podname’ 时还是提示找不到，见b步骤

b、删掉 ~/Library/Caches/CocoaPods 目录下面的 search_index.json 文件
pod setup 成功后，依然搜索不到，是因为之前你执行了 pod search时生成了 search_index.json 这个文件，这个文件在那个时候是没有你的库的，所以要删掉

c、删掉后，在执行 pod search ‘podname’
这个时候会输出 creating search index for spec repo 'master'.. Done!
可能会等一小会，就会出现你的要 search 的 pod 库了。

