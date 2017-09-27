# 准备工作
## 获取LinkedME Key

<font color="red">新用户</font>：在官网网站[注册账号](https://www.linkedme.cc/dashboard/index.html#/access/signup)，注册后[创建应用](https://www.linkedme.cc/dashboard/index.html#/app/aplt/create)，在后台导航栏“设置”中查看LinkedME Key。
<font color="red">老用户</font>：已经在官网网站注册账号，直接[创建应用](https://www.linkedme.cc/dashboard/index.html#/app/aplt/create)（可以创建多个应用），直接到导航栏“设置”中查看LinkedME Key。


## 获取LinkedME iOS SDK及Demo
到官方网站下载LinkedME-iOS-Deep-Linking-SDK，下载[Demo工程](https://github.com/WFC-LinkedME/LinkedME-iOS-Deep-Linking-Demo)

# 导入SDK
您可以直接导入下载的SDK或通过Cocoapods安装SDK

## 直接导入LinkedME iOS SDK
把Demo工程中的LinkedME_iOS.framework，导入工程中。


```
CoreSpotlight.framework (status:Optional)
SystemConfiguration.framework
Security.framework
WebKit.framework
StoreKit.framework
```

												
<font color="red">注意事项：CoreSpotlight.framework必须标记为可选</font>。

## 通过Cocoapods安装SDK
如果您想更方便地集成/更新 LinkPage的SDK，可以使用Cocoapods工具，想要了解Cocoapods，推荐参考官方文档[《CocoaPods安装和使用教程》](http://code4app.com/article/cocoapods-install-usage)。

* 在Podfile文件中添加  

**取IDFA版**(取IDFA为了广告效果检测,和统计相关,强烈建议集成带IDFA版本)
[集成IDFA版但是app中没有广告审核问题](https://github.com/WFC-LinkedME/LinkedME-iOS-Deep-Linking-Demo/blob/master/IDFA_Audit.md)
							
```
pod 'LinkedME-iOS-Deep-Linking-Demo_Pod_IDFA',
:git=>"https://github.com/WFC-LinkedME/LinkedME-iOS-Deep-Linking-Demo.git"
```	
							
**不取IDFA版**

														
```
pod 'LinkedME-iOS-Deep-Linking-Demo_Pod',
:git=>"https://github.com/WFC-LinkedME/LinkedME-iOS-Deep-Linking-Demo.git"
```	
							
* 运行 pod instal
* 从现在开始使用 .xcworkspace 打开项目，而不是 .xcodeproj


# 基本配置


## 添加系统Framework


* CoreSpotlight.framework(status:Optional)
* SystemConfiguration.framework
* Security.framework  
* WebKit.framework
* StoreKit.framework


注意：<font color="red">CoreSpotlight.framework必须标记为可选</font>。

![](/assets/4.1.4.jpg)

## 配置linkedme_key
* 打开info.plist文件
* 在列表中点击右键选择add row添加一个分组
* 创建一个新的item名称为linkedme_key，类型为Dictionary
* 在linkedme_key新增一个字符串类型的item，live字段，到后台<font color="red">“设置”->“应用”</font>中进行查看

![](/assets/4.1.7.jpg)

## 配置URL Scheme
配置URL Scheme，以便通过URL Scheme来唤起APP
* 打开info.plist
* 找到URL Types（如果没有就右键add row添加一个）
* 添加"you app"(你的app的唯一标识字符串)

![](/assets/4.1.5.jpg)


## 配置Universal Link支持 (仅支持iOS 9)
配置Universal Link，使得iOS9中可以通过Universal Link来唤起APP
* 在左侧导航器中点击您的项目
* 选择'Capabilities'标签
* 打开'Associated Domains'开关
* 添加applinks:lkme.cc和applinks:www.lkme.cc

![](/assets/4.1.6.jpg)

## 添加URLScheme和Universal Link支持
在SDK中配置URL Scheme和Universal Link，使得可以通过URL Scheme和Universal Link唤起APP
在Appdelegate中实现下列方法：

{% codetabs name="Objective-C", type="C" -%}

- (BOOL)application:(UIApplication*)application openURL:(NSURL*)url sourceApplication:(NSString*)sourceApplication annotation:(id)annotation{
  //判断是否是通过LinkedME的UrlScheme唤起App
  if ([[url description] rangeOfString:@"click_id"].location != NSNotFound) {
    return [[LinkedME getInstance] handleDeepLink:url];
  }

  return YES;
}

//Universal Links 通用链接实现深度链接技术
- (BOOL)application:(UIApplication*)application continueUserActivity:(NSUserActivity*)userActivity restorationHandler:(void (^)(NSArray*))restorationHandler{

  //判断是否是通过LinkedME的Universal Links唤起App
  if ([[userActivity.webpageURL description] rangeOfString:@"lkme.cc"].location != NSNotFound) {
    return  [[LinkedME getInstance] continueUserActivity:userActivity];
  }
  return YES;
}

//URI Scheme 实现深度链接技术
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary *)options{
  NSLog(@"opened app from URL %@", [url description]);

  //判断是否是通过LinkedME的UrlScheme唤起App
  if ([[url description] rangeOfString:@"click_id"].location != NSNotFound) {
    return [[LinkedME getInstance] handleDeepLink:url];
  }
  return YES;
}
{%- language name="Swift", type="Swift" -%}
//URI Scheme 实现深度链接技术
func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject) -> Bool {
  //判断是否是通过LinkedME的UrlScheme唤起App
  if url.absoluteString.componentsSeparatedByString("click_id").count > 1 {
    return LinkedME.getInstance().handleDeepLink(url);
  }
}

//Universal Links 通用链接实现深度链接技术
func application(application: UIApplication, continueUserActivity userActivity: NSUserActivity, restorationHandler: ([AnyObject]?) -> Void) -> Bool {
  //判断是否是通过LinkedME的Universal Links唤起App
  if url.absoluteString.componentsSeparatedByString("lkme.cc").count > 1 {
    return LinkedME.getInstance().continueUserActivity(userActivity);
  }
}

//URI Scheme 实现深度链接技术
func  application(app: UIApplication, openURL url: NSURL, options: [String : AnyObject]) -> Bool {
  //判断是否是通过LinkedME的UrlScheme唤起App
  if url.absoluteString.componentsSeparatedByString("click_id").count > 1 {
    return LinkedME.getInstance().handleDeepLink(url);
  }
}

{%- endcodetabs %}





# 深度链接功能
本模块实现的功能是创建深度链接及通过深度链接跳转到APP内的详情页面，若想要使用如下功能，请务必将“基本配置”部分全部实现

## 创建深度链接
<font color="red">温馨提示：如果web端集成了web sdk，则无需客户端创建深度链接，本节无需集成</font>。  
通过SDK创建深度链接，例如在分享页面时，页面的链接是通过SDK生成的深度链接，当打开分享内容时就可以通过深度链接唤起APP并进入对应页面


```java
//创建短链
-(void)addPara{
  self.linkedUniversalObject = [[LMUniversalObject alloc] init];
  self.linkedUniversalObject.title = title;//标题
  LMLinkProperties *linkProperties = [[LMLinkProperties alloc] init];
  linkProperties.channel = @"";//渠道(微信,微博,QQ,等...)
  linkProperties.feature = @"Share";//特点
  linkProperties.tags=@[@"LinkedME",@"Demo"];//标签
  linkProperties.stage = @"Live";//阶段
  [linkProperties addControlParam:@"View" withValue:arr[page][@"url"]];//页面唯一标识
  [linkProperties addControlParam:@"LinkedME" withValue:@"Demo"];//Demo标识 
  //开始请求短链
  [self.linkedUniversalObject getShortUrlWithLinkProperties:linkProperties andCallback:^(NSString *url, NSError *err) {
    if (url) {
      NSLog(@"[LinkedME Info] SDK creates the url is:%@", url);
      //拼接连接
      [H5_LIVE_URL stringByAppendingString:arr[page][@"form"]];
      [H5_LIVE_URL stringByAppendingString:@"?linkedme="];
      H5_LIVE_URL = [NSString stringWithFormat:@"https://www.linkedme.cc/h5/%@?linkedme=",arr[page][@"form"]];
      //前面是Html5页面,后面拼上深度链接https://xxxxx.xxx (html5 页面地址) ?linkedme=(深度链接)
      //https://www.linkedme.cc/h5/feature?linkedme=https://lkme.cc/AfC/mj9H87tk7
      LINKEDME_SHORT_URL = [H5_LIVE_URL stringByAppendingString:url];      
    } else {
      LINKEDME_SHORT_URL = H5_LIVE_URL;
    }
  }];
}
```


|参数名称|含义|功能|
|---|---|---|
|Channel|渠道|表示深度链接的渠道，方便统计分析和追踪，例如微信、微博，百度等等；|
|Feature|特点|表示深度链接的特点，例如邀请，分享等等；|
|Tags|标签|表示深度链接的标签特性，自定义任何值；|
|Stage|阶段|表示深度链接的阶段特性，比如第一版产品发布，第二版本测试等等；|

## 解析深度链接
通过深度链接唤起APP时，解析深度链接携带的参数以打开对应页面

### 配置AppDelegate

Objective-C请先进行如下配置：
1. 在AppDelegate中引入头文件`#import <LinkedME_iOS/LinkedME.h>`
2. 在Appdelegate里注册ViewController

Swift请先进行如下配置：  
1. 在xxxx-Bridging-Header.h中导入头文件`#import <LinkedME_iOS/LinkedME.h>`
2. 在Appdelegate里注册ViewController


### 配置注册ViewController设置及跳转方式

{% codetabs name="Objective-c", type="C" -%}

	
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
// Override point for customization after application launch.'
//初始化及实例
LinkedME* linkedme = [LinkedME getInstance];

//    //注册需要跳转的viewController
UIStoryboard * storyBoard=[UIStoryboard storyboardWithName:@"Main" bundle:[NSBundle mainBundle]];
DetailViewController  *dvc=[storyBoard instantiateViewControllerWithIdentifier:@"detailView"];

//[自动跳转]如果使用自动跳转需要注册viewController
//    [linkedme registerDeepLinkController:featureVC forKey:@"LMFeatureViewController"];

//获取跳转参数
[linkedme initSessionWithLaunchOptions:launchOptions automaticallyDisplayDeepLinkController:NO deepLinkHandler:^(NSDictionary* params, NSError* error) {
  if (!error) {
    //防止传递参数出错取不到数据,导致App崩溃这里一定要用try catch
    @try {
      NSLog(@"LinkedME finished init with params = %@",[params description]);
      //获取标题
      NSString *title = [params objectForKey:@"$og_title"];
      NSString *tag = params[@"$control"][@"View"];

      if (title.length >0 && tag.length >0) {
//如果app需要登录或者注册后，才能打开详情页，这里可以先把值存起来，登录/注册完成后，再使用
        //[自动跳转]使用自动跳转
        //SDK提供的跳转方法
        /**
        *  pushViewController : 类名
        *  storyBoardID : 需要跳转的页面的storyBoardID
        *  animated : 是否开启动画
        *  customValue : 传参
        *
        *warning  需要在被跳转页中实现次方法 - (void)configureControlWithData:(NSDictionary *)data;
        */

        //  [LinkedME pushViewController:title storyBoardID:@"detailView" animated:YES customValue:@{@"tag":tag} completion:^{
        ////
        //  }];

        //自定义跳转
        dvc.openUrl = params[@"$control"][@"ViewId"];
        [[LinkedME getViewController] showViewController:dvc sender:nil];
      }
    } @catch (NSException *exception) {

    } @finally {
    }
  } else {
    NSLog(@"LinkedME failed init: %@", error);
  }
}];
  return YES;
}
{%- language name="Swift", type="Swift" -%}
	
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
  // Override point for customization after application launch.
  let linkedme = LinkedME.getInstance();

  //是否开启Debug模式,开启Debug模式将会打印日志,上线时请关闭Debug模式
  //linkedme.setDebug();

  let storyBoard = UIStoryboard().instantiateViewControllerWithIdentifier("detailView");
  linkedme.registerDeepLinkController(storyBoard, forKey: "detailView");

  //解析深度链获取跳转参数，开发者自己实现参数相对应的页面内容。
  linkedme.initSessionWithLaunchOptions(launchOptions, automaticallyDisplayDeepLinkController: false) { (params, error) in
    if(error != nil){
      print("LinkedME finished init with params\(params.description)");
       let title = params["$og_title"];
       //如一个电商类的App通过商品ID来判断和区分改进入哪个详情页
       let goodsID = params["$control"]!["goodsID"];
       
       if (title!.isEqualToString("DetailViewController")){
       //页面跳转,使用自动跳转方式必须在被跳转的页面中实现代理方法传值,通过customValue传一个字典
       LinkedME.pushViewController("这里填写需要跳转的类名" as! String, storyBoardID: "这里填写StoryBoardID", animated: true, customValue: goodsID as! [AnyObject], completion: {});
	  /*
	   * 注:如果不是使用StoryBoard创建的View使用下面的方法进行跳转,更多方法进入LinkedME.h中查看
       * + (void)pushViewController:(NSString *)vc animated: (BOOL)flag customValue:(NSDictionary *)dict completion:(void (^)(void))completion NS_AVAILABLE_IOS(5_0);
       * 或者自己获取xxx.navigationController跳转页面,使用属性传值,如果没有使用自动跳转方法就不用注册View和实现代理方法.
       */
      }
    }else{
       print(error);
    }
  }
  return true

{%- endcodetabs %}







# 用户行为追踪
Track是追踪用户APP内行为的工具，这里的用户行为既可以是APP内页面浏览的次数，也可以是启动APP的次数，还可以是用户在APP内的任何点击行为，例如图片下载、音乐播放、文章分享等，甚至是APP内某种逻辑的判断，都可以通过Track来追踪。
针对不同行为，您还可以添加具体的特征描述，例如下载图片的类型，播放音乐的流派，分享文章的作者等。

## 使用Track功能
Track功能适用于iOS 6.0及以上操作系统的设备。系统提供三个标准效果点，如果不能满足您的监测需求，还可以自定义效果点

- 激活用户  
LinkedME AdTracking数据系统中的“用户”，指用户的一台唯一设备。 激活用户，指下载、安装并首次启动应用成功的用户
- 注册用户  
在应用中，通过提交信息，注册成功的用户。
- 自定义效果点  
指用户在应用中进行了特定的操作或达成了特定的条件。 例如：用户点击了广告栏、用户打通了某个关卡等。 自定义效果点用于收集任意您期望跟踪的数据

### 注册
  
  在用户帐号注册成功的时候调用LMTracking的onRegister方法：
  
```java
+ (void)onRegister:(NSString *)account;

```

|参数|类型|描述|
|---|---|---|
|Account|NSString|用户账号|

示例代码：

```java
[LMTracking onRegister:@"Your_userId"];
```

### 登录
  
  在用户帐号登录成功的时候调用 LMTracking 的 onLogin 方法:

```java
+ (void)onLogin:(NSString *)account;
```

|参数|类型|描述|
|---|---|---|
|Account|NSString|用户账号|


示例代码：
```java
[LMTracking onLogin:@"Your_userId"];
```

### 应用内支付
  
  在用户帐号登录成功的时候调用 LMTracking 的 onLogin 方法：

```java
+ (void)onPay:(NSString *)payAccount withOrderId:(NSString *)orderId orderDetail:(NSDictionary *)orderDetail withAmount:(int)amount withAccount:(NSString *)account;
```

|参数|类型|描述|
|---|---|---|
|payAccount|NSString|支付帐号|
|orderId|NSString|订单ID，最多64个字符，全局唯一，由开发者提供并维护（此ID很重要，如果不清楚集成时咨询客服）。用于唯一标识一次交易，以及后期系统之间对账使用；*如果多次充值成功的orderID重复，将只计算首次成功的数据，其他数据会认为重复数据丢弃。|
|amount|int|订单金额，单位为所选货币的分。比如:600分或100美分，币种以后面的currrencyType为标准。|
|orderDetail|NSDictionary|订单详情，自定义字段字典，如： @{@"name":@"iPhone",@"color":@"Black"}|
|account|NSString|用户账号|

示例代码：

```java
NSDictionary * dict = @{@"name":@"iPhone",@"color":@"Black"};
[LMTracking onPay:@"user001" withOrderId:@"30012" orderDetail:dict withAmount:88 withAccount:@"user"];
```

### 添加自定义效果
  
  自定义效果点，在需要的时候调用`LMTracking的+ (void)onCustEvent:(NSString *)eventName;` 方法：

```java
+ (void)onCustEvent:(NSString *)pointName pointProperties:(NSDictionary *)pointProperties userAccount:(NSString *)account;
```

|参数|类型|描述|
|---|---|---|
|pointName|NSString|自定义效果点名称|
|pointProperties|NSDictionary|自定义字段字典，自定义字段的字典格式，如： @{@"custom_info":@"xxx",@"upgrade":@"1"}|
|account|NSString|用户名|

示例代码：

```java
NSDictionary * dict = @{@"Name":@"xiaowang",@"Age":@"11"};
[LMTracking onCustEvent:@"custom1" pointProperties:dict userAccount:@"LKME.CC"];
```

### 验证接口

当SDK成功向服务器传输数据时，会有类似下边的日志输出：

```java
2016-11-10 12:18:12.990 LinkedMEiOSExample[38392:1830193] LMTrackingDataSDK:Start sending data. 2016-11-10 12:18:13.089 LinkedMEiOSExample[38392:1855022] LMTrackingDataSDK:Send data success!
```

# 其他功能
## Debug模式
在Debug模式下会打印日志

{% codetabs name="Objective-c", type="C" -%}
[linkedme setDebug];
{%- language name="Swift", type="Swift" -%}
linkedme.setDebug();
{%- endcodetabs %}

## 测试模式
若想测试集成SDK后是否能正确生成并解析深度链接，可以使用测试模式。测试模式需要先在后台中注册您的测试设备，测试设备产生的数据将进入测试系统（Test）中。

- 在后台(Dashboard)中-设置-测试-添加测试设备

<font color="red">OC</font>：通过`[LinkedME getTestID]`获取设备ID,去后台中添加设备  
<font color="red">Swift</font>：通过`LinkedME.getTestID()`获取设备ID,去后台中添加设备

## Spotlight 索引
配置Spotlight索引后，可以在iPhone的系统级搜索（主屏下拉或下拉菜单中的搜索）中搜索内容并直接打开APP的特定页面

### 创建Spotlight索引

{% codetabs name="Objective-c", type="C" -%}
[[LinkedME getInstance] createDiscoverableContentWithTitle:@"LinkedME 国内第一家企业级深度链接"
			description:@"让APP不再是信息孤岛!"
			thumbnailUrl:[NSURL URLWithString:@"http://7xq8b0.com1.z0.glb.clouddn.com/logo.png"]
			linkParams:dict
			type:@""
			publiclyIndexable:NO keywords:set5
			expirationDate:nil
			spotlightIdentifier:@"bbcc"
			spotlightCallback:^(NSString *url, NSString *spotlightIdentifier, NSError *error) {

			}];
{%- language name="Swift", type="Swift" -%}
LinkedME.getInstance().createDiscoverableContentWithTitle("LinkedME 国内第一家企业级深度链接",
			description: "让APP不再是信息孤岛!",
			thumbnailUrl: NSURL.init(string: "http://7xq8b0.com1.z0.glb.clouddn.com/logo.png"),
			linkParams: dic,
			type: nil,
			publiclyIndexable: false,
			keywords: keyWord as NSSet as Set,
				expirationDate: nil,
				spotlightIdentifier: "linkedme") { (url, spotlightID, error) in
		}
{%- endcodetabs %}

**设置关键字**

{% codetabs name="Objective-c", type="C" -%}
NSSet *keyWord = [NSSet setWithObjects:@"linkedme", nil];
{%- language name="Swift", type="Swift" -%}
let keyWord = NSSet.init(array: ["linkedme","hellolkm"])
{%- endcodetabs %}

**需要传递的参数**

{% codetabs name="Objective-c", type="C" -%}
NSSet *set5 = [NSSet    setWithObjects:@"linkedme",@"linked",@"深度链接", nil];
{%- language name="Swift", type="Swift" -%}
let dic = ["url":"http://linkedme.cc"]
{%- endcodetabs %}

**关键字详解**

|title|标题|
|---|---|
|description|描述|
|publiclyIndexable|是否公开|
|type|类型|
|thumbnailUrl|缩略图Url|
|keywords|关键字|
|userInfo|用户详情|
|expirationDate|失效日期,设置失效日期会自动删除索引|
|identifier|标志符|
|callback|回调|
|spotlightCallback|Spotlight回调|


### 删除索引

**删除所有索引**
{% codetabs name="Objective-c", type="C" -%}
[LinkedME removeAllSearchItems];
{%- language name="Swift", type="Swift" -%}
LinkedME.removeAllSearchItems();
{%- endcodetabs %}

**通过spotlightIdentifier删除索引**
{% codetabs name="Objective-c", type="C" -%}
[LinkedME removeSearchItemWith:@[@"linkedme"]];
{%- language name="Swift", type="Swift" -%}
LinkedME.removeSearchItemWith(["linkedme"]);
{%- endcodetabs %}

