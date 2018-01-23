## charles的简单使用

[charles 官网](https://www.charlesproxy.com/documentation/getting-started/)


### 安装

没钱买正版就找破解版，或者使用30试用

### 开启chareles后，无法抓到任何包

通常情况下，打开charles，然后菜单栏选择Proxy -> Mac OS X Proxy 即可，接着所有访问的请求都可以在charles中看到。

但是我这边碰到一个问题，就是我选择了这个，却还是抓不到请求。查阅资料发现，原因是我系统设置了vpn代理导致。

见下图

![](https://huangzhifei.github.com/images/vpn-set.png)

因为我使用的 ss FQ工具会自动代理，所以导致 charles 无法抓包，我解决的办法是抓包的时候关掉了 ss，有人说可以设置让vpn和charles共存，我懒得弄了。

### 抓取HTTPS时，response是乱码

1、安装SSL证书

去 http://www.charlesproxy.com/ssl.zip 下载CA证书文件。

2、解压该zip文件后，双击其中的.crt文件，这时候在弹出的菜单中选择“总是信任”

3、从钥匙串访问中即可看到添加成功的证书

4、菜单栏 Proxy -> SSL Proxyin Settings -> add，如下图

![](https://huangzhifei.github.com/images/charles-https.png)

配置完这个后，https 的请求就可以捕捉到了。


### Charles 2种封包的视图，分别名为“Structure”和"Sequence"

1、Structure视图将网络请求按访问的域名分类。

2、Sequence视图将网络请求按访问的时间排序。

3、过滤可以使用 find 功能，或者在 proxy setting 加永久的过滤规则

### 截取iPhone上的网络封包

这个功能直接看下面链接 [](http://www.infoq.com/cn/articles/network-packet-analysis-tool-charles)

总结就是要让手机的网络和电脑的在一个路由下面，让电脑成为其代理。

