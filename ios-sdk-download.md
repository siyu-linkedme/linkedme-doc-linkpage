

# iOS SDK及Demo下载
[SDK下载](https://github.com/WFC-LinkedME/LinkedME-iOS-Deep-Linking-Demo)

[Demo下载](https://github.com/WFC-LinkedME/LinkedME-iOS-Deep-Linking-Demo)
 
# iOS SDK更新日志
## 版本V1.3.3
发布日期：2016年12月09日  
<font color="red">**改善**</font>
* 优化深度链接使用体验，通过深度链接进入app点击右上角lime.cc域名深度链接不失效
 
## 版本V1.3.2
发布日期：2016年11月30日  
<font color="red">**新增**</font>
* SDK全面使用https链接
* 添加Tracking功能
* 添加有米SimulateIDFA支持

## 版本V1.0.1
发布日期：2016年4月7日  
<font color="red">**新增**</font>  
* 在iOS上优化Deferred Deep Linking技术，由getSafariCookie接口，提高匹配精度；
* 在iPhone上支持Universal Links技术；iOS9.0以上版本，支持Universal Links，iOS9.0以下版本，支持URI Scheme；
* 在iOS-Deep-Linking-SDK中不再获取IDFA，以免触犯用户隐私；
* 添加Debug模式，方便开发者提供调试SDK；
* 添加Retry Number和Timeout接口，方便开发者设置连接次数和网络请求超时；
* 添加Spotlight搜索功能；  

<font color="red">**改善**</font>  
* 修改网络请求的消息队列机制，处理异步请求和同步请求，降低网络包的大小；
* 搭建Restful接口，添加Mcq消息队列机制、Mysql动态扩容、优化Redis和MC缓存等技术方案；

## 版本V1.0.0
发布日期：2015年7月1日  
<font color="red">**新增**</font>  
* 在iOS上实现Deferred Deep Linking技术，以DeviceFingerprintId和BrowserFingerprintId技术进行匹配；
* 使用URI Scheme方式实现Deep Linking技术；
* 支持Android，iOS，iPad跨平台，支持多种浏览器，比如Chrome，Firefox等等；
* 搭建网络请求队列机制，支持同步及异步请求方式；
* 改善SDK类的继承结构，优化代码结构降低代码量，降低包的大小；