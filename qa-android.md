# Android平台相关问题

#### **Q1: 不同浏览器的兼容问题？**

A1:  LinkedME支持市场上主流的浏览器，比如Chrome、UC、360浏览器、默认浏览器、猎豹浏览器、搜狗浏览器等等。

#### **Q2: 微信中如何唤起APP并跳转到指定页面？**

A2:  可通过两种方式：  
1. 通过应用宝+deferred deep linking实现；  
2. 通过“在浏览器中打开”使用外部浏览器通过URI Scheme的方式唤起APP实现。

#### **Q3: Android 6.0及以上支持App Links，各个分享平台的支持情况如何？ **

A3:  首先，App Links需要验证成功才能将指定的应用作为唤起特定域名的默认应用，且只会在安装和升级的时候验证，无论验证成功或失败都不会尝试多次验证； 其次，分享到的应用平台（例如微信）不能对分享链接进行特殊处理。  
只有满足以上条件才能真正实现App Links，但是目前国内的情况是：  
1. 部分手机无法验证成功！经过多次深入测试，推断为含有Google服务的手机在不翻墙的情况下，无法验证成功，不含有Google服务的手机均可验证成功。  
2. 多数软件（例如：微信、微博、QQ）对所有http/https链接进行了处理，无法使用App Links功能，短信、邮箱可以。

#### **Q4: 想要通过应用宝跳转，需要什么额外条件吗？**

A4:  要想通过应用宝跳转，应用必须在应用宝市场上线并开通了“微下载”功能，且移动端必须安装了最新的App，才可实现流畅跳转。

#### **Q5: 对各个浏览器的支持情况 **

A5:  除百度浏览器V7.0\(基于Blink内核\)开始的版本外，其他浏览器均支持良好。

#### **Q6: 如何开启App Links **

A6:  开启方法：使用java的 keytool 命令获取SHA256指纹证书  
1. windows  
打开cmd命令行工具，输入 java -version 查看Java版本号，若无法查看请先配置Java环境变量，若显示Java版本号，则在cmd中继续输入命令 keytool -list -v -keystore 发布apk的签名文件路径\(以.keystore或.jks为结尾\)  
示例：

```
keytool -list -v -keystore C:\Users\Administrator\Desktop\debug.keystore
```

![](/assets/docs_QA_windows.png)

2.mac  

打开终端，输入 java -version 查看Java版本号，若无法查看请先配置Java环境变量，若显示Java版本号，则在终端中继续输入命令 keytool -list -v -keystore 发布apk的签名文件路径\(以.keystore或.jks为结尾\)  

示例：

```
keytool -list -v -keystore Desktop/debug.keystore
```

![](/assets/docs_QA_mac.png)


#### **Q7: 如何开通应用宝微下载功能？**

A7:   登录[腾讯开放平台](http://open.qq.com)，打开应用设置页面，选择“运营服务”下的“微下载”功能进行设置。
![](/assets/aaa.png)
![](/assets/bbb.png)





