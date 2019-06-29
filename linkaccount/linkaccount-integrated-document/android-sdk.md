# Android SDK

## 准备工作

#### SDK说明

* 一键登录与号码认证服务需要设备开启蜂窝数据网络，与运营商基站通信;
* 一键登录与号码认证（目前支持中国移动2/3/4G、中国联通3/4G、中国电信4G）目前SDK提供aar包集成；
* 支持网络环境为  

  a.纯蜂窝数据网络  

  b.蜂窝数据网络与wifi网络双开

* 对于双卡手机，取当前流量卡号；
* SDK支持的最低版本为15；

#### 创建应用

应用的创建流程及APPID/APPKEY的获取，请查看「[产品指南](../product-guide/)」文档

## SDK集成

### **SDK导入及配置**

**LinkAccount SDK 获取**

从官网下载aar包，将aar包拷贝至工程的libs目录下。

**配置AndroidManifest.xml**

**配置权限**

```markup
<!--允许应用程序联网，用于访问网关和认证服务器-->
<uses-permission android:name="android.permission.INTERNET"/>
<!--获取imsi用于判断双卡和换卡-->
<uses-permission android:name="android.permission.READ_PHONE_STATE"/>
<!--允许程序访问WiFi网络状态信息-->
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
<!--获取网络状态，判断是否数据、wifi等-->
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<!--允许程序改变网络连接状态-->
<uses-permission android:name="android.permission.CHANGE_NETWORK_STATE"/>
<!--地理位置信息-->
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
```

| 权限 | 说明 | 使用说明 |
| :--- | :--- | :--- |
| INTERNET | 允许应用程序联网 | 用于访问网关和认证服务器 |
| READ\_PHONE\_STATE | 允许读取手机状态 | 获取imsi用于判断双卡和换卡 |
| ACCESS\_WIFI\_STATE | 允许程序访问WiFi网络状态信息 | 判断当前网络环境 |
| ACCESS\_NETWORK\_STATE | 获取网络状态 | 判断是否数据、wifi等 |
| CHANGE\_NETWORK\_STATE | 允许程序改变网络连接状态 | 当处于wifi，强行切换使用数据网络 |
| ACCESS\_COARSE\_LOCATION | 地理位置信息 | 基站相关 |

**声明Activity**

在AndroidManifest.xml文件中声明必要的Activity

```markup
 <!-- LinkAccount start -->
        <activity
                android:name="cc.lkme.linkaccount.LoginAuthActivity"
                android:configChanges="orientation|keyboardHidden|screenSize"
                android:screenOrientation="portrait"
                android:launchMode="singleTop">
        </activity>
<!-- LinkAccount end -->

<!--中国移动 start-->
        <activity
                android:name="com.cmic.sso.sdk.activity.LoginAuthActivity"
                android:configChanges="orientation|keyboardHidden|screenSize"
                android:screenOrientation="portrait"
                android:launchMode="singleTop">
        </activity>
<!--中国移动 end-->
```

**混淆规则**

```text
 # 联通取号、认证混淆
 -dontwarn com.unicom.xiaowo.login.**
 -keep class com.unicom.xiaowo.login.**{*;}

# 移动混淆
 -dontwarn com.cmic.sso.sdk.**
 -keep class com.cmic.sso.sdk.**{*;}

# 电信混淆
 -dontwarn cn.com.chinatelecom.account.**
 -keep class cn.com.chinatelecom.account.**{*;}

# LinkAccount
 -dontwarn cc.lkme.linkaccount.**
 -keep class cc.lkme.linkaccount.**{*;}
```

### **SDK 初始化**

**接口**

```java
/**
 * 初始化LinkAccount SDK
 *
 * @param context  Application Context
 * @param key      linkaccount key
 * @param listener TokenResultListener 结果监听
 * @return LinkAccount 实例
 */
public static LinkAccount getInstance(@NonNull Context context, @NonNull String key, TokenResultListener listener)
```

**说明**

* 只需初始化一次，多次调用不会多次初始化，与一次调用效果一致；

**参数描述**

| 参数 | 是否必填 | 类型 | 说明 |
| :--- | :--- | ---: | ---: |
| context | 是 | Context | Application Context |
| key | 是 | String | 后台分配的应用APPkey |
| tokenResultListener | 是 | TokenResultListener | 预取号、一键登录、号码认证回调结果监听 |

**示例代码**

```java
// 初始化
LinkAccount.getInstance(getApplicationContext(), "linkaccount key", new TokenResultListener() {
    @Override
    public void onSuccess(@AbilityType final int resultType, final String tokenJson, final String originResult) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                switch (resultType) {
                    case AbilityType.ABILITY_ACCESS_CODE:
                         break;
                    case AbilityType.ABILITY_TOKEN:
                         LinkAccount.getInstance().quitAuthActivity();
                         try {
                            JSONObject jsonObject = new JSONObject(tokenJson);
                            String accessToken = jsonObject.optString("accessToken");
                            String operatorType = jsonObject.optString("operatorType");
                            String gwAuth = jsonObject.optString("gwAuth");
                            String os = jsonObject.optString("os");
                            } catch (JSONException e) {
                                e.printStackTrace();
                            }
                        break;
                    case AbilityType.ABILITY_MOBILE_TOKEN:
                         break;
                  }
            }
        });
    }

     @Override
     public void onFailed(int resultType, final String info) {
         runOnUiThread(new Runnable() {
             @Override
             public void run() {
                 Toast.makeText(MainActivity.this, info, ToaNGTH_SHORT).show();
             }
         });

     }
 });
```

**结果监听**

```java
/**
 * 预取号、一键登录、号码认证结果回调监听
 */
public interface TokenResultListener {
    /**
     * 获取成功，返回token
     *
     * @param resultType   结果类型 0: 预取号结果 1: 一键登录结果 2: 号码认证结果
     * @param tokenJson    结果json字符串数据
     * @param originResult 运营商返回的原始数据
     */
    void onSuccess(int resultType, String tokenJson, String originResult);

    /**
     * 失败，返回失败的结果，json串格式
     *
     * @param resultType 错误类型
     * @param result     错误说明
     */
    void onFailed(int resultType, String result);
}
```

**说明**

用于预取号、一键登录、号码认证结果回调监听；根据返回的不同的结果类型需要分别处理。

**参数描述**

成功回调

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| resultType | int | 结果类型 0: 预取号结果 1: 一键登录结果 2: 号码认证结果 |
| tokenJson | String | 结果Json数据，需要用户自己解析结果 |
| originResult | String | 运营商返回的原始数据 |

tokenJson说明

```javascript
{
    "resultCode":6666, // 成功状态码
    "operatorType": "CM", // CM: 中国移动 CU: 中国联通 CT: 中国电信 XX: 未知
    "accessToken": "llllllll", // 一键登录或号码认证 token，移动、联通、电信均返回
    "gwAuth": "6789", // 一键登录或号码认证 auth，电信返回
    "os": "1" // 系统标识，1: Android
}
```

失败回调

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| resultType | int | 结果类型 0: 预取号结果 1: 一键登录结果 2: 号码认证结果 |
| result | String | 错误描述，参见文档后面错误码描述 |

### **预取号**

**接口**

```java
/**
* 预取号，必须先调用该方法，才可调用一键登录与号码认证方法
* 移动手机号必须申请电话权限，否则无法预取号成功
*
* @param timeout 预取号超时时间，2000-8000ms
*/
public void preLogin(int timeout)
```

**说明**

* 移动手机号必须有电话权限，否则无法预取号及后续操作，建议提前获取电话权限；
* 预取号是一键登录与号码认证调用的前提条件；
* 回调结果在初始化SDK中的TokenResultListener中监听；
* 调用该方法后，不建议立即调用一键登录方法；

**使用场景**

* 初始化SDK后，当用户有意向注册或登录时调用预登录接口，可加快一键登录授权页面的调起速度；

**参数描述**

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| timeout | int | 请求超时时间（毫秒），可设置2000-8000毫秒 |

**示例代码**

```java
 // 预取号
 LinkAccount.getInstance().preLogin(5000);
```

### **一键登录（拉起授权页面）**

**接口**

```java
/**
 * 一键登录，获取一键登录的token，然后回传自己服务器置换手机号
 *
 * @param timeout 超时时间，2000-8000ms
 */
public void getLoginToken(int timeout)
```

**说明**

* 调用前必须首先调用预取号接口；
* 回调结果在初始化SDK中的TokenResultListener中监听。

**使用场景**

* 调用一键登录方法，SDK将会拉起授权页面，用户授权后，SDK将返回用于置换手机号的 token 给到应用客户端，客户端将该 token （电信同时需要gwAuth）传给自己的应用服务器，用来置换手机号。

**参数描述**

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| timeout | int | 请求超时时间（毫秒），可设置2000-8000毫秒 |

**示例代码**

```java
// 一键登录
LinkAccount.getInstance().getLoginToken(5000);
```

### **号码认证**

**接口**

```java
/**
 * 号码认证，认证用户填写的手机号是否是本机号码
 *
 * @param timeout 预取号超时时间，2000-8000ms
 */
public void getMobileCode(int timeout)
```

**说明**

* 调用前必须首先调用预取号接口；
* 回调结果在初始化SDK中的TokenResultListener中监听。

**使用场景**

* 调用号码认证方法，SDK将返回用于手机号认证的 token 给到应用客户端，客户端将该 token 及用户填写的手机号传给自己的应用服务器，用来验证填写的号码是否是本机号码。

**参数描述**

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| timeout | int | 请求超时时间（毫秒），可设置2000-8000毫秒 |

**示例代码**

```java
// 号码认证
LinkAccount.getInstance().getMobileCode(5000);
```

### **手动关闭授权页面**

**接口**

```java
/**
 * 关闭授权页面
 */
public void quitAuthActivity()
```

**说明**

* 手动关闭一键登录页面

**示例代码**

```java
// 关闭授权页面
LinkAccount.getInstance().quitAuthActivity();
```

### **打印日志**

**接口**

```java
/**
 * 是否打印相关认证日志
 *
 * @param isDebug true: 打印 false: 不打印
 */
public void setDebug(boolean isDebug)
```

**参数描述**

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| isDebug | boolean | 是否打印相关日志 |

**示例代码**

```java
// 打印日志
LinkAccount.getInstance().setDebug(true);
```

## **一键登录授权页面修改**

![&#x6388;&#x6743;&#x9875;&#x9762;&#x8BBE;&#x8BA1;&#x89C4;&#x8303;](../../.gitbook/assets/sdk-shou-quan-ye-she-ji-gui-fan%20%281%29.jpg)

**接口**

```java
/**
 * 一键登录授权页面配置
 *
 * @param authUIConfig 布局配置
 */
public void setAuthUIConfig(AuthUIConfig authUIConfig)
```

```java
 /**
  * ①设置导航栏背景色
  */
 public AuthUIConfig.Builder setNavColor(int navColor)
 /**
  * ②设置导航栏标题内容
  */
 public AuthUIConfig.Builder setNavText(String navText)
 /**
  * 设置导航栏标题字体大小
  */
 public AuthUIConfig.Builder setNavTextSize(int navTextSize)

 /**
  * 设置导航栏标题颜色
  */
 public AuthUIConfig.Builder setNavTextColor(int navTextColor)
 /**
  * ③设置返回按钮图片
  */
 public AuthUIConfig.Builder setNavBackImgPath(String navBackImgPath)
 /**
  * ④设置logo图片
  */
 public AuthUIConfig.Builder setLogoImgPath(String logoImgPath)
 /**
  * 设置是否隐藏logo
  */
 public AuthUIConfig.Builder setLogoHidden(boolean logoHidden)
 /**
  * 设置logo padding top
  */
 public AuthUIConfig.Builder setLogoPaddingTop(int logoPaddingTop)
 /**
  * 设置logo padding bottom
  */
 public AuthUIConfig.Builder setLogoPaddingBottom(int logoPaddingBottom)
 /**
  * 设置logo图标宽度
  */
 public AuthUIConfig.Builder setLogoWidth(int logoWidth)
 /**
  * 设置logo图标高度
  */
 public AuthUIConfig.Builder setLogoHeight(int logoHeight)
 /**
  * ⑤设置脱敏号码颜色
  */
 public AuthUIConfig.Builder setNumberColor(int numberColor)
 /**
  * 设置脱敏号码大小
  */
 public AuthUIConfig.Builder setNumberSize(int numberSize)
 /**
  * ⑥设置slogan字体颜色
  */
 public AuthUIConfig.Builder setSloganTextColor(int sloganTextColor)
 /**
  * ⑦设置登录按钮文字
  */
 public AuthUIConfig.Builder setLogBtnText(String logBtnText)
 /**
  * 设置登录按钮字体颜色
  */
 public AuthUIConfig.Builder setLogBtnTextColor(int logBtnTextColor)
 /**
  * 设置登录按钮字体大小
  */
 public AuthUIConfig.Builder setLogBtnTextSize(int logBtnTextSize)
 /**
  * 设置登录按钮高度
  */
 public AuthUIConfig.Builder setLogBtnHeight(int logBtnHeight)
 /**
  * 设置登录按钮背景图片
  */
 public AuthUIConfig.Builder setLogBtnBackgroundPath(String logBtnBackgroundPath)
 /**
  * ⑨设置隐私条款1的文字和链接
  */
 public AuthUIConfig.Builder setAppPrivacyOne(String name, String url)
 /**
  * 设置隐私条款2的文字和链接
  */
 public AuthUIConfig.Builder setAppPrivacyTwo(String name, String url)
 /**
  * 设置隐私条款基础颜色和条款颜色
  */
 public AuthUIConfig.Builder setAppPrivacyColor(int baseColor, int privacyColor)
 /**
  * ⑧设置切换方式视图是否可见
  */
 public AuthUIConfig.Builder setSwitchHidden(boolean switchHidden)  
 /**
  * 设置切换方式视图字体颜色
  */
 public AuthUIConfig.Builder setSwitchTextColor(int switchTextColor)
 /**
  * 设置切换方式视图的点击事件
  */
 public AuthUIConfig.Builder setSwitchClicker(View.OnClickListener switchClicker)
```

**参数描述**

| 参数 | 类型 | 说明 |
| :--- | :--- | :--- |
| authUIConfig | AuthUIConfig | 一键登录授权页面配置 |

**说明**

* 配置一键登录授权页面需要在调用一键登录方法之前调用才可生效；

**示例代码**

```java
AuthUIConfig.Builder builder = new AuthUIConfig.Builder();
builder.setNavText("欢迎登录");
LinkAccount.getInstance().setAuthUIConfig(builder.create());
```

## **返回码对照**

| 返回码 | 说明 |
| :--- | :--- |
| 6666 | 成功 |
| 10000 | 未知错误 |
| 10001 | SDK未初始化 |
| 10002 | 未检测到网络访问权限 |
| 10003 | 初始化失败 |
| 10004 | LinkedAccount Key无效 |
| 10005 | 预取号失败 |
| 10006 | Token获取失败 |
| 10007 | 号码认证Token获取失败 |
| 10008 | 监听未初始化 |
| 10009 | json格式化数据异常 |
| 10010 | 用户取消 |
| 10011 | 请先调用预取号接口 |
| 10012 | 一键登录切换到其他方式 |
| 10013 | 未找到相应方法 |
| 10014 | 用户未授权READ\_PHONE\_STATE |

## 常见问题

**某些厂商机增加了移动数据网络权限设置问题**

由于某些手机操作系统增加了应用的数据网络权限，在手机wifi和数据网络同时打开时，应用首次打开默认使用wifi数据通道，此时应用的数据网络权限是关闭的，最终导致一键登录失败。

解决方法：用户须在纯移动数据网络环境打开应用，用户授权应用使用移动数据网络权限后SDK才能正常使用

**号码认证支持的移动网络情况**

中国移动支持2G/3G/4G、中国联通支持3G/4G、中国电信支持4G，但2G网络下认证失败率稍高。 

