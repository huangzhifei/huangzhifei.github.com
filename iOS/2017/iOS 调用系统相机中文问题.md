## iOS 调用系统相机中文问题-备忘

### 方法一
在 info.plist 里面增加 key-value 

Localized resources can be mixed = YES

### 方法二
在 info.plist 里面增加 key-value 

Localization native development region = China

以上两种方式都可以行

### iOS中调用相机要获取权限

Privacy - Camera Usage Description                   调用相机

Privacy - Photo Library Usage Description            调用相册