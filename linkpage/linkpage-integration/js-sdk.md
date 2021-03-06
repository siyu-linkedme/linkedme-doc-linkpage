# JS SDK

## 获取LinkedME Key

新用户：在官网网站[注册账号](https://www.linkedme.cc/dashboard/index.html#/access/signup)，注册后[创建应用](https://www.linkedme.cc/dashboard/index.html#/app/aplt/create)，在后台导航栏“设置”中查看LinkedME Key。

老用户：已经在官网网站注册账号，直接[创建应用](https://www.linkedme.cc/dashboard/index.html#/app/aplt/create)（可以创建多个应用），直接到导航栏“设置”中查看LinkedME Key。

如图：![](../../.gitbook/assets/qi-ye-wei-xin-jie-tu-e4bc32b9929045a9900c6031264bbbc1.png)

## 引入 JS SDK

在html文件的head标签里增加如下代码：

> 提示：JS SDK会不定期优化跳转逻辑，请不要将JS SDK下载到本地加载。

```javascript
  <script src="https://static.lkme.cc/linkedme.min.js" ></script>
```

## 在HTML页面中准备一个或多个用于打开深度链接的a元素

> 开发者可根据自身需求调整，如需要创建多个深度链接，就需要建立多个a标签

```text
<a id="open_header" href="">打开LinkedME深度链接</a>
<a id="open_footer" href="">打开LinkedME深度链接</a>
```

**注意：新版本已经不需要给a标签添加class名"linkedme"，如果您的代码中包含class="linkedme"，请去除并参照下面步骤检查集成代码（尤其是一个页面使用多个深度链接的用户）。**

## JS SDK 集成配置

> 提示：请在服务器环境下测试，本地打开存在跨域问题。

### 初始化LinkedME全局对象

#### 接口名称: init\(linkedme\_key, initData, callback\)

参数说明：

```javascript
linkedme_key 【必选】
类型: String
每个app会分配唯一一个linkedme key，用户在linkedme官网创建app之后可以在设置菜单里面找到linkedme_key，请参照步骤1，把值粘贴于此处

initData  【必选】
类型：Object
初始化linkedme对象参数

callback【可选】
类型：Function
回调函数,处理初始化成功或者失败后的逻辑
```

示例代码：

```javascript
<script type="text/javascript">
  var initData = {};
  initData.type = "live";
  linkedme.init("linkedme_key", initData, null);
</script>
```

### 创建深度链接

> 注意：下面步骤实现的功能是创建深度链接及通过深度链接跳转到APP内的详情页面，若想要使用如下功能，请务必实现上面1.4.1的内容

#### 接口名称： link\(data, callback, autoSelect\)

参数说明：

```javascript
    data 【必选】
    类型：Object
    生成深度链接的请求参数

    data.promotion_name【可选】
    类型：String
    自定义深度链接名称

    data.feature 【可选】
    类型：String
    自定义深度链接功能名称，多个名称用逗号分隔

    data.stage 【可选】
    类型：String
    自定义深度链接阶段名称，多个名称用逗号分隔

    data.channel【可选】
    类型：String
    自定义深度链接渠道名称，多个名称用逗号分隔

    data.tags【可选】
    类型：String
    自定义深度链接标签名称，多个名称用逗号分隔

    data.ios_custom_url【可选】
    类型：String
    限制长度为512个字符以内，首次集成测试请留空！自定义iOS平台下App的下载地址，如果是AppStore的下载地址可以不用填写，需填写http或https。注意不能使用.apk地址,建议填写H5地址。

    data.ios_direct_open【可选】
    类型：String
    首次集成测试请默认为false！未安装情况下直接打开ios_custom_url

    data.android_custom_url【可选】
    类型：String
    首次集成测试请留空！！！自定义安卓平台下App的下载地址，需填写http或https。注意不能使用apk地址，建议填写H5地址。

    data.android_direct_open【可选】
    类型：String
    首次集成测试请默认为false！未安装情况下直接打开android_custom_url

    data.params【可选】
    类型：JSON字符串
    跳转到详情页面的所需参数，如'{"key1":"value1","key2":"value2"...}'

    callback【必选】
    类型：Function
    回调函数：
    1.生成深度链接成功，深度链接可以通过response.url得到;       
    2.调用linkedme.trigger_deeplink(response.url)
    3.同时把response.url赋值于a标签的href属性，将深度链接绑定到<a>标签

    autoSelect【可选】
    类型：String
    生成深度链接之后是否自动打开深度链接，默认值为false
```

示例代码：

```javascript
<script>
  var data = {};
  data.type = "live";
  data.promotion_name = "链接名称";
  data.feature = "功能名称";
  data.stage = "阶段名称"; 
  data.channel = "渠道名称"; 
  data.tags = "标签名称"; 
  data.ios_custom_url = ""; 
  data.ios_direct_open = "false";
  data.android_custom_url = "";
  data.android_direct_open = "false";
  var options = {
        "key1":"value1",
        "key2":"value2",
        ...
   }
  data.params = JSON.stringify(options); 
  linkedme.link(data, function(err, response) {
    if (err) {
      // 生成深度链接失败，返回错误对象err
    } else {
        var open_header = document.getElementById("open_header");
        open_header.addEventListener("click",function(){
         linkedme.trigger_deeplink(response.url);
        })
       open_header.setAttribute('href', response.url);
    }
  },false);

  var datafooter = {};  //创建第二个深度链接所需参数
  ...
  linkedme.link(datafooter, function(err, response) {
    if (err) {

    } else {
        var open_footer = document.getElementById("open_footer");
        open_footer.addEventListener("click",function(){
         linkedme.trigger_deeplink(response.url);
        })
       open_footer.setAttribute('href', response.url);
    }
  },false);
</script>
```

> 请使用a标签作为打开app的跳转按钮，参照步骤1.3

## 功能测试

测试跳转链接地址：

[https://www.linkedme.cc/standard/sample.html](https://www.linkedme.cc/standard/sample.html)

或者自行完成功能测试。

如仍有问题可加qq群639389757咨询，我司免费提供前期接入的技术支持服务。

## 常见问题FAQ

> 注：请严格按照以上集成文档进行集成。

Q1：使用的前端框架不习惯于直接操作DOM，可不可以使用window.location方式来打开a标签？

A：不建议使用，会出现IOS不兼容的情况。

Q2：修改ios\_custom\_url、ios\_direct\_open、android\_custom\_url、android\_direct\_open这四个参数为什么没有生效，还是原来的跳转逻辑？

A：只修改上述几个参数，并不会重新创建生成深度链接，也就不会生效，[详情请点击](https://pagedoc.lkme.cc/linkpage/qa/qa-web.html#q2-%E9%80%9A%E8%BF%87js-sdk%E6%9B%B4%E6%94%B9%E5%8F%82%E6%95%B0%E5%90%8E%EF%BC%8C%E5%B9%B6%E6%B2%A1%E6%9C%89%E7%94%9F%E6%95%88%EF%BC%8C%E5%90%8C%E6%97%B6%E4%B9%9F%E6%B2%A1%E6%9C%89%E9%87%8D%E6%96%B0%E7%94%9F%E6%88%90%E6%B7%B1%E5%BA%A6%E9%93%BE%E6%8E%A5)  


