{% codetabs name="Python", type="py" -%}
msg = "Hello World"
print msg
{%- language name="JavaScript", type="js" -%}
var msg = "Hello World";
console.log(msg);
{%- language name="HTML", type="html" -%}
<b>Hello World</b>
{%- endcodetabs %}





{% codetabs name="Objective-c", type="Objective-c" -%}
msg = "Hello World"
print msg
{%- language name="Swift", type="Swift" -%}
<b>Hello World</b>
{%- endcodetabs %}





## 配置Universal Link支持 (仅支持iOS 9)
配置Universal Link，使得iOS9中可以通过Universal Link来唤起APP
* 在左侧导航器中点击您的项目
* 选择'Capabilities'标签
* 打开'Associated Domains'开关
* 添加applinks:lkme.cc和applinks:www.lkme.cc
![](https://www.linkedme.cc/docs/images/4.1.6.jpg)
## 添加URLScheme和Universal Link支持
在SDK中配置URL Scheme和Universal Link，使得可以通过URL Scheme和Universal Link唤起APP
在Appdelegate中实现下列方法



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
<font color="red">温馨提示：如果web端集成了web sdk，则无需客户端创建深度链接，本节无需集成。</font>  
通过SDK创建深度链接，例如在分享页面时，页面的链接是通过SDK生成的深度链接，当打开分享内容时就可以通过深度链接唤起APP并进入对应页面








