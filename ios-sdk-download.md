

# iOS SDK及Demo下载
[SDK及Demo下载](https://github.com/WFC-LinkedME/LinkedME-iOS-Deep-Linking-Demo/archive/master.zip)
 
# iOS SDK更新日志

## 版本V1.5.2.2
发布日期：2018年01月16日  
<font color="red">**改善**</font>
* 1.修复了1.5.2.1版本中的一个bug；
* 2.优化陈旧机型可能会获取数据慢的问题；
* 3.优化新用户全新下载后跳转到之前浏览的页面功能


## 版本V1.5.1.8
发布日期：2017年12月22日  
<font color="red">**改善**</font>
* 深度链接优化前期SDK准备工作;

## 版本V1.4.8.8
发布日期：2017年3月22日  
<font color="red">**改善**</font>
* 修复部分iOS 6机型崩溃bug
* 修复iPhone 6s模拟器崩溃bug

## 版本V1.4.8.5
发布日期：2017年3月22日  
<font color="red">**改善**</font>
* 修复一个创建短链失败的bug

## 版本V1.4.8
发布日期：2017年3月17日  
<font color="red">**改善**</font>
* 优化跳转体验
* 优化网络请求

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