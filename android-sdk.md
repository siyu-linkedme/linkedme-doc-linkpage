# 准备工作
## 获取LinkedME Key
<font color="red">新用户</font>：在官网网站[注册账号](https://www.linkedme.cc/dashboard/index.html#/access/signup)，注册后[创建应用](https://www.linkedme.cc/dashboard/index.html#/app/aplt/create)，在后台导航栏“设置”中查看LinkedME Key。
<font color="red">老用户</font>：已经在官网网站注册账号，直接[创建应用](https://www.linkedme.cc/dashboard/index.html#/app/aplt/create)（可以创建多个应用），直接到导航栏“设置”中查看LinkedME Key。

## 获取LinkedME Android SDK及Demo
到官方网站下载LinkedME-Android-Deep-Linking-SDK，下载 [Demo工程](https://github.com/WFC-LinkedME/LinkedME-Android-Deep-Linking-Demo)，获取工程libs目录下的 [LinkedME-Android-Deep-Linking-SDK-V1.0.*.jar](https://github.com/WFC-LinkedME/LinkedME-Android-Deep-Linking-Demo/tree/master/LinkedME-Demo/libs)支持包。
# 导入SDK
## 导入LinkedME Android SDK
支持两种方式添加支持库引用：

**方式一：下载jar包并导入**  
把下载的LinkedME-Android-Deep-Linking-SDK-V1.0.*.jar文件放到项目libs文件夹下，并添加到项目Module层的build.gradle依赖中,如下所示:
```java
dependencies {
  //注意修改jar包名,与下载的jar包名称一致
  compile files('libs/LinkedME-Android-Deep-Linking-SDK-V1.0.16.jar')
}

```

**方式二：添加maven仓库引用导入**
* 在工程根节点的build.gradle中添加maven仓库地址，如下所示:

```java
buildscript {
    repositories {
        jcenter()
        maven {
            url 'https://dl.bintray.com/linkedme2016/lkme-deeplinks'

        }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.3.0'
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {

    repositories {
        jcenter()
        maven {
            url 'https://dl.bintray.com/linkedme2016/lkme-deeplinks'
        }
    }
}
```

* 在项目Module层的build.gradle中添加依赖，如下所示：

```java
dependencies {
compile fileTree(include: ['*.jar'], dir: 'libs')
compile "cc.linkedme.deeplinks:link-page:1.0.16"
}
```


# 基本配置
## 配置AndroidManifest.xml

### 添加LinkedME Key


```java
<application
  android:name=".activity.LinkedMEDemoApp">
  <!-- LinkedME官网注册应用后,从"设置"页面获取该Key -->
  <meta-data
    android:name="linkedme.sdk.key"
    android:value="替换为后台设置页面中的LinkedME Key" />
</application>
```


### 添加访问权限
集成LinkedME SDK需要开启的访问权限，权限说明如下表格所示：

|权限|用途|
|------|--------|
|android.permission.INTERNET|	访问网络|
|android.permission.READ_PHONE_STATE	|获取电话信息，为了获取手机的IMEI号|
|android.permission.ACCESS_NETWORK_STATE	|获取网络状态，是否联网|
|android.permission.ACCESS_WIFI_STATE	|获取WiFi状态|
|android.permission.WRITE_EXTERNAL_STORAGE	|写入外部存储|
|android.permission.BLUETOOTH	|获取设备名称|

添加代码如下：


```java
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
  <!--LinkedME SDK 需要开启的权限-->  <!--LinkedME SDK 需要开启的权限-->
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.READ_PHONE_STATE" />
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
  <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  <uses-permission android:name="android.permission.BLUETOOTH" />
</manifest>
```


### 添加URI Scheme和App Links支持
添加URI Scheme和App Links支持后，才能够通过这两种方式跳转到APP内
在工程主页的Activity中添加`android:launchMode="singleTask"`属性。

* URI Scheme方式；
* App Links方式；  

注意事项：
1. 修改android:scheme；请在后台“设置”->“链接”中查看Android下的URI Scheme的值；
2. 修改android:pathPrefix；请在后台“设置”->“概览”中查看LinkedME App ID的值；
LinkedME-Android-Deep-Linking-Demo代码如下所示：


```xml
<application android:name=".activity.LinkedMEDemoApp">
  <activity
    android:name=".activity.MainActivity"
    android:configChanges="orientation|keyboard"
    android:label="@string/app_name"
    android:launchMode="singleTask"
    android:screenOrientation="portrait">

    <!-- URI Scheme方式 在dashboard配置中,请保持与ios的URI Scheme相同 -->
    <!--
    如果程序已存在与此完全相同的data配置,即只包含scheme并且值完全相同,
    则需要考虑发起的intent会出现相同应用图标的选择对话框的情况
    参考集成文档:https://www.linkedme.cc/docs/page4.html#link1
    -->
    <intent-filter>
      <!-- 此处scheme值需要替换为后台设置中的scheme值 -->
      <!-- host禁止更改！！！ -->
      <!-- 禁止配置其他属性 -->
      <data android:scheme="lkmedemo"
            android:host="linkedme" />
      <action android:name="android.intent.action.VIEW" />
      <category android:name="android.intent.category.DEFAULT" />
      <category android:name="android.intent.category.BROWSABLE" />
    </intent-filter>

    <!-- APP Links方式,Android 23版本及以后支持 -->
    <intent-filter android:autoVerify="true">
      <action android:name="android.intent.action.VIEW" />
      <category android:name="android.intent.category.DEFAULT" />
      <category android:name="android.intent.category.BROWSABLE" />
      <!-- 以下pathPrefix值需要替换为后台设置中 App ID 的值-->
      <!-- host中设置的lkme.cc不要更改！！！-->
      <data
        android:host="lkme.cc"
        android:pathPrefix="/AfC"
        android:scheme="https" />
      <data
        android:host="lkme.cc"
        android:pathPrefix="/AfC"
        android:scheme="http" />
    </intent-filter>
  </activity>
</application>
```

## 初始化LinkedME实例
在自定义Application类中的<font color="red">onCreate()</font>方法中，添加初始化LinkedME实例的代码。LinkedME-Android-Deep-Linking-Demo示例代码如下所示：


```java
public class LinkedMEDemoApp extends Application {
  @Override
  public void onCreate() {
    super.onCreate();
    
    // 初始化SDK
    LinkedME.getInstance(this);

    if (BuildConfig.DEBUG) {
       //设置debug模式下打印LinkedME日志
       LinkedME.getInstance().setDebug();
     }   
    //初始时设置为false，在配置Uri Scheme的Activity的onResume()中设置为true
    LinkedME.getInstance().setImmediate(false);
       
    }
}  

```


若应用需要向前兼容到Android 4.0以下版本，请在<font color="red">基类</font>（如：BaseActivity）中添加如下代码以便管理Session：


```java
public class BaseActivity extends AppCompatActivity {

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        //兼容14之前的版本需要在基类中添加以下代码
        LinkedME.getInstance().onLMCreated(this);
        super.onCreate(savedInstanceState);
    }

    @Override
    protected void onStart() {
        //兼容14之前的版本需要在基类中添加以下代码
        LinkedME.getInstance().onLMStarted(this);
        super.onStart();
    }

    @Override
    protected void onResume() {
        //兼容14之前的版本需要在基类中添加以下代码
        LinkedME.getInstance().onLMResumed(this);
        super.onResume();
    }

    @Override
    protected void onPause() {
        //兼容14之前的版本需要在基类中添加以下代码
        LinkedME.getInstance().onLMPaused(this);
        super.onPause();
    }

    @Override
    public void onStop() {
        //兼容14之前的版本需要在基类中添加以下代码
        LinkedME.getInstance().onLMStoped(this);
        super.onStop();
    }

    @Override
    protected void onDestroy() {
        //兼容14之前的版本需要在基类中添加以下代码
        LinkedME.getInstance().onLMDestoryed(this);
        super.onDestroy();
    }
}
```


## 配置URI Scheme唤起的Activity页面(例如：MainActivity)
添加以下配置保证APP能正常跳转到特定详情页面。

```java
 // 添加此处目的是针对后台APP通过uri scheme唤起的情况，
    // 注意：即使不区分用户是否登录也需要添加此设置，也可以添加到基类中
    @Override
    protected void onNewIntent(Intent intent) {
        setIntent(intent);
    }
```
在onResume()中方法调用LinkedME.getInstance().setImmediate(true)方法，开启跳转功能，从而控制从主页面跳转到指定页面。 示例如下：


```java
 @Override
    protected void onResume() {
        super.onResume();
        LinkedME.getInstance().setImmediate(true);
    }
```


# 深度链接功能
本模块实现的功能是创建深度链接及通过深度链接跳转到APP内的详情页面，若想要使用如下功能，请务必将“基本配置”部分全部实现
## 创建深度链接
> 温馨提示：如果web端集成了js sdk，则无需客户端创建深度链接，本节无需集成。(建议采用js sdk创建深度链接)  

通过SDK创建深度链接，例如在分享页面时，页面的链接是通过SDK生成的深度链接，当打开分享内容时就可以通过深度链接唤起APP并进入对应页面。
LinkedME SDK创建深度链接，必须传入链接的参数，用于区分App内不同的页面。比如唯品会商品详情页面的唯一标识为productId=230453452

|参数名称|含义|功能|
|---|---|---|
|Channel|渠道|表示深度链接的渠道，方便统计分析和追踪，例如微信、微博，百度等等；|
|Feature|特点|表示深度链接的特点，例如邀请，分享等等；|
|Tags|标签|表示深度链接的标签特性，自定义任何值；|
|Stage|阶段|表示深度链接的阶段特性，比如第一版产品发布，第二版本测试等等；|
示例代码如下：


```java
public class ShareActivity extends BaseActivity {

  public void share() {
    /**创建深度链接*/
    //web服务器无法创建深度链接时,客户端可选择创建
    //深度链接属性设置
    LinkProperties properties = new LinkProperties();
    //渠道
    properties.setChannel("");  //微信、微博、QQ
    //功能
    properties.setFeature("Share");
    //标签
    properties.addTag("LinkedME");
    properties.addTag("Demo");
    //阶段
    properties.setStage("Live");
    //设置该h5_url目的是为了iOS点击右上角lkme.cc时跳转的地址，一般设置为当前分享页面的地址
    //客户端创建深度链接请设置该字段
    properties.setH5Url("https://linkedme.cc/h5/feature");
    //自定义参数,用于在深度链接跳转后获取该数据
    properties.addControlParameter("LinkedME", "Demo");
    properties.addControlParameter("View", loadUrl);
    LMUniversalObject universalObject = new LMUniversalObject();
    universalObject.setTitle(title);
    // 异步生成深度链接
    universalObject.generateShortUrl(ShareActivity.this, properties, new LMLinkCreateListener() {
      //https://www.lkme.cc/AfC/idFsW02l7
      @Override
      public void onLinkCreate(final String url, LMError error) {
       if (error == null) {
        Log.i("linkedme", "创建深度链接成功！创建的深度链接为：" + url);
        //deepLinkUrl创建返回的深度链接
        final UMImage image = new UMImage(ShareActivity.this, "https://www.linkedme.cc/homepage2.jpg");
        /**友盟分享化分享，分享的链接不单单是H5链接，而是携带深度链接的H5链接*/
        new ShareAction(ShareActivity.this).setDisplayList(SHARE_MEDIA.WEIXIN).setShareboardclickCallback(new ShareBoardlistener() {
          @Override
          public void onclick(SnsPlatform snsPlatform, SHARE_MEDIA share_media) {

            if (share_media == SHARE_MEDIA.WEIXIN) {
              //微信
              new ShareAction(ShareActivity.this)
              .setPlatform(share_media)
              .withText(shareContent)
              .withTitle("LinkedME" + title)
              .withMedia(image)
              //拼接深度链接,客户端将生成的深度链接值拼接到链接后
              .withTargetUrl(H5_URL + url_path + "?linkedme=" + url)
              .setCallback(umShareListener)
              .share();
            }
          }
        }).open();
        }else{
         Log.i("linkedme", "创建深度链接失败！失败原因：" + error.getMessage());
        }
      }
    });
  }
}
```
> 提示：虽然客户端可自行创建深度链接并分享，但是web端也需要对分享链接进行处理才可使用深度链接，需要将分享链接中的深度链接截取出来，并作为“打开app”按钮的跳转链接(因此，建议使用js sdk创建深度链接)。例如：
原有的分享链接为：https://www.linkedme.cc/h5/partner 
追加深度链接的分享链接为：https://www.linkedme.cc/h5/partner?linkedme=https://lkme.cc/AfC/CeG9o5VH8
web端需要将深度链接https://lkme.cc/AfC/CeG9o5VH8取出并作为“打开app”按钮的跳转链接。


## 解析深度链接
通过深度链接唤起APP时，解析深度链接携带的参数以打开对应页面
新建一个Activity(例如：MiddleActivity)，用于接收SDK回传的参数，并根据业务要求进行跳转
- 首先，创建MiddleActivity，并在AndroidManifest.xml中配置MiddleActivity  
    1. 添加属性：`android:noHistory="true"`，目的是不显现该页面也不让其放入栈中，只进行页面逻辑跳转；
    2. 添加配置，`<meta-data android:name="linkedme.sdk.auto_link_keys" android:value="linkedme"/>`，目的是为了让SDK可以将参数回传给该页面(请不要更改value值！);
LinkedME-Android-Deep-Linking-Demo的MiddleActivity在AndroidManifest.xml中的示例代码如下所示：


```xml
<activity
      android:name=".activity.MiddleActivity"
      android:screenOrientation="portrait"
      android:noHistory="true">
      <meta-data android:name="linkedme.sdk.auto_link_keys" android:value="linkedme"/>
  </activity>
```


* 其次，在MiddleActivity的onCreate()方法中编写跳转逻辑
    1. 通过getIntent().getParcelableExtra(LinkedME.LM_LINKPROPERTIES)获取跳转参数
LinkedME-Android-Deep-Linking-Demo的MiddleActivity示例代码如下所示：


```java
public class MiddleActivity extends AppCompatActivity {
    // ...
/**
     * 
解析深度链获取跳转参数，开发者自己实现参数相对应的页面内容

     * 
通过LinkProperties对象调用getControlParams方法获取自定义参数的HashMap对象,
     * 通过创建的自定义key获取相应的值,用于数据处理。

     */
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        if (getIntent() != null) {
            //获取与深度链接相关的值
            LinkProperties linkProperties = getIntent().getParcelableExtra(LinkedME.LM_LINKPROPERTIES);
            if (linkProperties != null) {
                Log.i("LinkedME-Demo", "Channel " + linkProperties.getChannel());
                Log.i("LinkedME-Demo", "control params " + linkProperties.getControlParams());
                Log.i("LinkedME-Demo", "link(深度链接) " + linkProperties.getLMLink());
                Log.i("LinkedME-Demo", "是否为新安装 " + linkProperties.isLMNewUser());
                //获取自定义参数封装成的hashmap对象,参数键值对由集成方定义
                HashMap<String, String> hashMap = linkProperties.getControlParams();
                //根据key获取传入的参数的值,该key关键字View可为任意值,由集成方规定,请与web端商议,一致即可
                String view = hashMap.get("View");
                if (view != null) {
                    //根据不同的参数进行页面跳转,detail代表具体跳转到哪个页面,此处语义指详情页
                    if (view.equals("detail")) {
                        //DetailActivity类不存在,此处语义指要跳转的详情页,参数也是由上面的HashMap对象指定
                        Intent intent = new intent(MiddleActivity.this, DetailActivity.class);
                        startActivity(intent);
                    }
                }
            }
        }
        finish();
    }
    // ...
    }
```


# 用户行为追踪
Track是追踪用户APP内行为的工具，这里的用户行为既可以是APP内页面浏览的次数，也可以是启动APP的次数，还可以是用户在APP内的任何点击行为，例如图片下载、音乐播放、文章分享等，甚至是APP内某种逻辑的判断，都可以通过Track来追踪。
针对不同行为，您还可以添加具体的特征描述，例如下载图片的类型，播放音乐的流派，分享文章的作者等。
## 使用Track功能
Track功能适用于Android 2.3及以上操作系统的设备。系统提供三个标准效果点，如果不能满足您的监测需求，还可以自定义效果点
* 用户注册  
</br>在应用中，通过提交信息，注册成功的用户。
* 用户登录  
</br>在应用中，登录成功的用户。
* 用户付费  
</br>在应用中完成了支付、充值、消费等行为，并产生了交易金额的用户。
* 自定义效果点  
</br>指用户在应用中进行了特定的操作或达成了特定的条件。例如：用户点击了广告栏、用户打通了某个关卡等。自定义效果点用于收集任意您期望跟踪的数据


### 注册

用户帐号注册成功后调用LMTracking的onRegister()方法


```java
 public static void onRegister(String account)
```


|参数|类型|描述|
|---|---|---|
|Account|NSString|用户账号|
示例代码 :


```java
LMTracking.onRegister("Your_userId");
```


### 登录

在用户帐号登录成功的时候调用 LMTracking 的 onLogin 方法：


```java
public static void onLogin(String account)
```


|参数|类型|描述|
|---|---|---|
|Account|NSString|用户账号|
示例代码：


```java
LMTracking.onLogin("Your_userId");
```


### 订单支付

用户在提交订单时调用LMTracking的onPay接口：


```java
public static void onPay(String pay_account, String order_id, JSONObject order_detail, String order_amount, String account) 
```


|参数|类型|描述|
|---|---|---|
|pay_account|String|当前支付的用户名|
|order_id|String|订单ID，开发者自定义的订单ID编号，每一个订单都有唯一编号|
|order_detail|JSONObject|订单详情，以JSONObject对象存储订单的相关信息|
|order_amount|String|支付总金额|
|account|String|账号名称|
示例代码：


```java
JSONObject orderObject = new JSONObject();
 try {
     orderObject.putOpt("商品名称", "商品一");
     orderObject.putOpt("单价","123");
 } catch (JSONException e) {
     e.printStackTrace();
 }
 LMTracking.onPay("LinkedME001", "132456789", orderObject, "123", "LinkedME");
 ```
 
 
### 自定义效果点

在需要的时候调用LMTracking的onCustEvent()方法


```java
public static void onCustEvent(String point_name, JSONObject point_properties, String account) 
```


|参数|类型|描述|
|---|---|---|
|point_name|String|自定义效果点名称|
|point_properties|JSONObject|效果点自定义KV， json格式, eg:{"名称":"名称","年龄":14}|
|account|String|账号名称|
示例代码：


```java
JSONObject eventObject = new JSONObject();
 try {
     eventObject.putOpt("属性1","123");
     eventObject.putOpt("属性2","456");
 } catch (JSONException e) {
     e.printStackTrace();
 }
 LMTracking.onCustEvent("自定义事件一", eventObject, "LinkedME");
```


### 验证接口

当SDK成功向服务器传输数据时，会有类似下边的日志输出：


```java
2016-11-10 12:18:12.990 LinkedME:Start sending data.
2016-11-10 12:18:13.089 LinkedME:Send data success!
```


# 其他功能
## 测试模式
若想测试集成SDK后是否能正确生成并解析深度链接，可以使用测试模式。测试模式需要先在后台中注册您的测试设备，测试设备产生的数据将进入测试系统（Test）中。
