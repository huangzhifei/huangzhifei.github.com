## push证书

可以参考百度的push sdk文档 [http://push.baidu.com/doc/ios/api]()


***下面是个人总结***

#### 1、正常创建 app id 记得添加 push notifications 的选项

#### 2、创建证书文件cert

创建证书的时候需要在本地生成对应的CSR文件，CSR文件从钥匙串 -> 证书助理 -> 从证书颁发机构请求证书
这一步一定要在自己电脑上实现，因为后续在导出 p12 文件的时候，如果不是自己电脑上生成的对应的key，是没
有办法导出 p12 文件的。
         
    
#### 3、导出对应的 p12 文件

看下面的图片：
![](https://huangzhifei.github.com/images/adx_open_cer.png)

第一步不要展开，导出对应的 p12 文件;
第二步展开，导出对应的key。

#### 4、通过命令转换 p12 到对应的 pem 文件

```
openssl pkcs12 -in xxx证书.p12 -out xxx_pro.pem -nodes
openssl pkcs12 -in xxx-key.p12 -out xxx_key.pem -nodes

```

#### 5、生成 Provisioning Profile 描述文件

正常生成 dev 与 dis 的描述文件下载，导入xcode就ok了。

#### 6、总结

配置完 push 证书后，相应的代码可以参见项目 PushDemo， 里面有讲解 ios 10 及以前的 push 在 app 中用代码怎么实现的。

