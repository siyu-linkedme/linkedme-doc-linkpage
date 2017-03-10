{% codetabs name="Python", type="py" -%}
msg = "Hello World"
print msg
{%- language name="JavaScript", type="js" -%}
var msg = "Hello World";
console.log(msg);
{%- language name="HTML", type="html" -%}
<b>Hello World</b>
{%- endcodetabs %}





{% codetabs name="Objective-c", type="C" -%}
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


{% codetabs name="Objective-c", type="C" -%}
1在AppDelegate中引入头文件
#import <LinkedME_iOS/LinkedME.h>
2在Appdelegate里注册ViewController
2.1 配置注册ViewController设置及跳转方式
复制代码
	
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
在xxxx-Bridging-Header.h中导入头文件
#import <LinkedME_iOS/LinkedME.h>
2在Appdelegate里注册ViewController
2.1 配置注册ViewController设置及跳转方式
复制代码
	
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









