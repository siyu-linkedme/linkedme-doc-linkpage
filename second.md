{% codetabs name="Python", type="py" -%}
msg = "Hello World"
print msg
{%- language name="JavaScript", type="js" -%}
var msg = "Hello World";
console.log(msg);
{%- language name="HTML", type="html" -%}
<b>Hello World</b>
{%- endcodetabs %}


、、、
{{ 'hello/world' | melchior('Python', 'JavaScript', 'HTML') }}
、、、




{% codetabs name="Objective-c", type="Objective-c" -%}
msg = "Hello World"
print msg
{%- language name="Swift", type="Swift" -%}
<b>Hello World</b>
{%- endcodetabs %}










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




