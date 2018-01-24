## XCode之dSYM文件

参考文章：bugly
[https://bugly.qq.com/docs/user-guide/symbol-configuration-ios/?v=1492997248592#dsym_1]()


### XCode编译后没有生成dSYM文件

XCode release 编译默认会生成 dSYM 文件， 而 Debug 编译默认不会生成，对应的 Xcode 配置如下：

	XCode -> Build Settings -> Code Generation -> Generate Debug Symbols -> Yes

	XCode -> Build Settings -> Build Option -> Debug Information Format -> DWARF with dSYM File
	
![](https://github.com/huangzhifei/huangzhifei.github.com/raw/master/images/xcode-setting.png)


### 开启Bitcode之后需要注意问题

在点 “Upload to App Store” 上传到 App Store 服务器的时候需要声明符号文件（dSYM文件）的生成

![](https://github.com/huangzhifei/huangzhifei.github.com/raw/master/images/BitcodedSYM.jpg)

### 存在路径

有很多方式去查看这个 dSYM 文件的路径，这里直接给定一个路径 /Users/<用户名>/Library/Developer/Xcode/Archives 目录下，对于每一个发布版本我们都很有必要保存对应的 Archives 文件。


