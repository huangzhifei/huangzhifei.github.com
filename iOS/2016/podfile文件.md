## podfile文件

### 单target的podfile文件

```
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '8.0'
#忽视pod三方库的warnings
inhibit_all_warnings!
target 'CocoaPodTestTarget' do

pod 'Reachability',  '~> 3.0.0'

end
```

###  多个target的podfile文件

```
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '8.0'
#忽视pod三方库的warnings
inhibit_all_warnings!
target 'CocoaPodTestTarget' do

pod 'Reachability',  '~> 3.0.0'

end

target 'Second' do

pod 'OpenUDID', '~> 1.0.0'

end
```

**do/end作为开始和结束标识符**

### Podfile管理pods依赖库版本

再引入依赖库时，需要显示或隐式注明引用的依赖库版本，常用具体写法和表示含义如下：

```
pod 'AFNetworking'      			//不显式指定依赖库版本，表示每次都获取最新版本
pod 'AFNetworking', '2.0'     	//只使用2.0版本
pod 'AFNetworking', '> 2.0'     	//使用高于2.0的版本
pod 'AFNetworking', '>= 2.0'     //使用大于或等于2.0的版本
pod 'AFNetworking', '< 2.0'     	//使用小于2.0的版本
pod 'AFNetworking', '<= 2.0'     //使用小于或等于2.0的版本
pod 'AFNetworking', '~> 0.1.2'   //使用大于等于0.1.2但小于0.2的版本
pod 'AFNetworking', '~>0.1'     	//使用大于等于0.1但小于1.0的版本
pod 'AFNetworking', '~>0'     	//高于0的版本，写这个限制和什么都不写是一个效果，都表示使用最新版本
```

### Podfile 引入特殊节点
有时我们需要引入依赖库指定的分支或节点，写法如下：

#### 引入master分支（默认）
	pod 'AFNetworking', :git => 'https://github.com/gowalla/AFNetworking.git'
	
#### 引入指定的分支
	pod 'AFNetworking', :git => 'https://github.com/gowalla/AFNetworking.git', :branch => 'dev'

#### 引入某个节点的代码
	pod 'AFNetworking', :git => 'https://github.com/gowalla/AFNetworking.git', :tag => '0.7.0'
	
#### 引入某个特殊的提交节点
	pod 'AFNetworking', :git => 'https://github.com/gowalla/AFNetworking.git', :commit => '082f8319af'