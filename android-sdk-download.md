# Android SDK及Demo下载
[SDK下载](https://github.com/WFC-LinkedME/LinkedME-Android-Deep-Linking-Demo/raw/master/LinkedME-Demo/libs/LinkedME-Android-Deep-Linking-SDK-V1.0.9.ja)
[Demo下载](https://github.com/WFC-LinkedME/LinkedME-Android-Deep-Linking-Demo)
 
# Android SDK更新日志
## 版本V1.0.9
发布日期：2016年01月23日  
**改善**  
* 优化了SDK处理逻辑;

## 版本V1.0.8
发布日期：2016年01月11日  
**新增**  
* SDK添加清除跳转参数的方法;    
**改善**  
* 优化了SDK某些机型重复跳转的问题;

## 版本V1.0.7
发布日期：2016年12月27日  
**改善**  
* 优化了SDK跳转逻辑;

## 版本V1.0.6
发布日期：2016年11月25日  
**新增**  
* 新的集成方案，更简洁，更稳定，扩展性更好；  
* demo增加定时广告及跳转受登录限制两种情况的解决方案；  
**改善**  
* 修复了重复调用close接口的问题，在这种情况下会出现跳转不成功的问题；  
* 优化代码逻辑；

## 版本V1.0.5
发布日期：2016年10月21日  
**新增**  
* 增加了扫描测试短链二维码唤起APP成功后，弹出“集成成功”提示的功能；  
* 支持maven仓库导入依赖库；  
**改善**  
* 优化了深度链接跳转模块；
* 优化代码，减少jar包大小；

## 版本V1.0.4
发布日期：2016年9月8日  
**新增**  
* LinkProperties类增加getLMLink()与isLMNewUser()方法，分别获取深度链接及标识是否为新安装用户。  
**改善**  
* 优化了SDK获取intent data数据的处理逻辑，解决了与应用自定义intent的冲突；
* 解决了参数设置中包含“&”符号，客户端无法创建深度链接的问题；

## 版本V1.0.3
发布日期：2016年8月14日  
**改善**  
* 废弃了test模式，全部使用live模式，因此无需在AndroidManifest.xml文件中配置linkedme.sdk.TestMode及linkedme.sdk.key.test相关的meta-data值；
* 可通过在后台添加测试设备来区分是否为测试设备；
* 打印日志原本只能在test模式下打印，因test已被废弃，要在live模式下打印日志可通过Application中设置setDebug()来打印；

## 版本V1.0.2
发布日期：2016年7月29日  
**改善**  
* 废弃了继承自Application的LMAPP类，用户无需继承LMAPP类，可直接在自定义的Application中初始化LinkedME对象；
* 解决了应用宝跳转唤起后台应用后，无法跳转到详情页的问题；

## 版本V1.0.1
发布日期：2016年4月7日  
**新增**  
* 在Android上实现Deferred Deep Linking技术，添加User-agent，screen size提高匹配精度；
* 在Android上支持AppLinks技术；Android6.0以上版本，支持AppLinks，Android所有版本，支持URI Scheme；
* 在Android-Deep-Linking-SDK中不再获取HardwardId和Google advertising，以免触犯用户隐私；
* 添加Debug模式，方便开发者提供调试SDK；
* 添加Retry Number和Timeout接口，方便开发者设置连接次数和网络请求超时；  
**改善**  
* 修改网络请求的消息队列机制，处理异步请求和同步请求，降低网络包的大小；
* 搭建Restful接口，添加Mcq消息队列机制、Mysql动态扩容、优化缓存等技术方案；

## 版本V1.0.0
发布日期：2015年7月1日  
**新增**  
* 在Android上实现Deferred Deep Linking技术，以DeviceFingerprintId和BrowserFingerprintId技术进行匹配；
* 使用URI Scheme方式实现Deep Linking技术；
* 支持Android，iOS，iPad跨平台，支持多种浏览器，比如Chrome，Firefox等等；
* 搭建网络请求队列机制，支持同步及异步请求方式；
* 改善SDK类的继承结构，优化代码结构降低代码量，降低包的大小；