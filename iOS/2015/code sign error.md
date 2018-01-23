## code sign error

Command /usr/bin/codesign failed with exit code 1

也就是一直 code sign error

其实就是你的钥匙串的权限的问题，可能在 xcode 编译的时候，弹出了要输入登陆密码来获取钥匙串权限的时候，你拒绝了，然后后面 xcode 就不在弹出这个窗口了，你始终没法去重新输入来获取你的权限，是不是很坑。

按照下面的方法就可以了

[https://support.apple.com/ro-ro/HT201609]()

其实就是强制重新修改钥匙串的密码来让他更新，这样xcode在编译后就会重新弹出框，输入正确密码就好了。

