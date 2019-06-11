# JS SDK

## 准备工作

### 获取LinkedME Key

新用户：在官网网站[注册账号](https://www.linkedme.cc/dashboard/index.html#/access/signup)，注册后[创建应用](https://www.linkedme.cc/dashboard/index.html#/app/aplt/create)，在后台导航栏“设置”中查看LinkedME Key。

老用户：已经在官网网站注册账号，直接[创建应用](https://www.linkedme.cc/dashboard/index.html#/app/aplt/create)（可以创建多个应用），直接到导航栏“设置”中查看LinkedME Key。

## 引入 JS SDK

在html文件的head标签里增加如下代码：

```javascript
  <!-- 加载js -->
  <script src="https://static.lkme.cc/linkedme.min.js" ></script>
```

> 提示：JS SDK会不定期优化跳转逻辑，请不要将JS SDK下载到本地加载。

## 基本配置

### 初始化LinkedME全局对象

注意：请在服务器环境下测试，本地打开存在跨域问题。

```javascript
/* 
  接口名称: init(linkedme_key, initData, callback)
  参数说明：
    linkedme_key: 每个app会分配唯一一个linkedme key，用户在linkedme官网创建app之后可以在设置菜单里面找到linkedme_key 【必选】
    data: 初始化linkedme对象参数，比如测试时需要添加data.type="test",上线时需要修改为"live",如果传null,默认为"live" 【可选】
    callback: 回调函数 【可选】
*/
```

示例代码1：

```javascript
<script>
  linkedme.init("linkedme_key", null, null);
</script>
```

示例代码2：

```javascript
<script>
  var initData = {};
  initData.type = "live";  //表示现在使用线上模式,如果填写"test", 表示测试模式.
  linkedme.init("linkedme_key", initData, function(err, response){
    if(err){
    // 初始化失败，返回错误对象err
    } else {
    // 初始化成功，可以不做处理
    }
  });
</script>
```

## 深度链接功能

本模块实现的功能是创建深度链接及通过深度链接跳转到APP内的详情页面，若想要使用如下功能，请务必实现“基本配置”中的内容

### 创建深度链接

通过js创建深度链接，例如在H5页面中通过js将该页面的深度链接写到“打开APP”按钮下

```javascript
/* 
  接口名称： link(data, callback, autoSelect)
  参数说明：
    data ：生成深度链接的请求参数【必选】
    callback ：回调函数 【必选】
    autoSelect ：生成深度链接之后是否自动打开深度链接，默认值为false 【可选】
*/
```

示例代码：

```javascript
<script>
  var data = {};
  data.type = "live";  //表示现在使用线上模式,如果填写"test", 表示测试模式.【可选】
  data.feature = "功能名称"; // 自定义深度链接功能，多个名称用逗号分隔，【可选】
  data.stage = "阶段名称"; // 自定义深度链接阶段，多个名称用逗号分隔，【可选】
  data.channel = "渠道名称"; // 自定义深度链接渠道，多个名称用逗号分隔，【可选】
  data.tags = "标签名称"; // 自定义深度链接标签，多个名称用逗号分隔，【可选】
  data.ios_custom_url = ""; // 首次集成测试请留空！！！自定义iOS平台下App的下载地址，如果是AppStore的下载地址可以不用填写，需填写http或https【可选】
  data.ios_direct_open = "false"; //首次集成测试请默认为false！！！true：未安装情况下直接打开ios_custom_url，默认为false【可选】
  data.android_custom_url = "";// 首次集成测试请留空！！！自定义安卓平台下App的下载地址，需填写http或https【可选】
  data.android_direct_open = ""; // 首次集成测试请默认为false！！！true:所有情况下跳转android_custom_url，不会走深度链接跳转打开APP的逻辑，默认为false【可选】
  // 下面是自定义深度链接参数，用户点击深度链接打开app之后，params的参数会通过LinkedME服务器透传给app，由app根据参数进行相关跳转
  // 例如：详情页面的参数，写入到params中，这样在唤起app并获取参数后app根据参数跳转到详情页面
  var value1 = 1;
  var value2 = 2;
  data.params = '{"key1":"'+value1+'","key2":"'+value2+'"}'; //注意单引号和双引号的位置
  linkedme.link(data, function(err, response) {
    if (err) {
      // 生成深度链接失败，返回错误对象err
    } else {
      // 生成深度链接成功，深度链接可以通过data.url得到
      response.url
    }
  },false);
</script>
```

> 注意：修改ios\_custom\_url、ios\_direct\_open、android\_custom\_url、android\_direct\_open这四个参数你会发现：点击深度链接还是走之前的逻辑，新设置的参数并没有生效，因为修改这四个值并不会重新创建深度链接，也就不会生效，具体请参考：[https://pagedoc.lkme.cc/qa-web.html\#q2-通过js-sdk更改参数后，并没有生效，同时也没有重新生成深度链接](https://pagedoc.lkme.cc/qa-web.html#q2-通过js-sdk更改参数后，并没有生效，同时也没有重新生成深度链接)

### 完整代码示例

```text
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script>
      linkedme.init("linkedme_key", {type: "live"}, null);
      var data = {};
      data.type = "live";  //表示现在使用线上模式,如果填写"test", 表示测试模式.【可选】
      data.feature = "功能名称"; // 自定义深度链接功能，多个用逗号分隔，【可选】
      data.stage = "阶段名称"; // 自定义深度链接阶段，多个用逗号分隔，【可选】
      data.channel = "渠道名称"; // 自定义深度链接渠道，多个用逗号分隔，【可选】
      data.tags = "标签名称"; // 自定义深度链接标签，多个用逗号分隔，【可选】
      data.ios_custom_url = ""; // 首次集成测试请留空！！！自定义iOS平台下App的下载地址，如果是AppStore的下载地址可以不用填写，需填写http或https【可选】
      data.ios_direct_open = "false"; //首次集成测试请默认为false！！！true：未安装情况下直接打开ios_custom_url，默认为false【可选】
      data.android_custom_url = "";// 首次集成测试请留空！！！自定义安卓平台下App的下载地址，需填写http或https【可选】
      data.android_direct_open = ""; // 首次集成测试请默认为false！！！true:所有情况下跳转android_custom_url，不会走深度链接跳转打开APP的逻辑，默认为false【可选】
      // 下面是自定义深度链接参数，用户点击深度链接打开app之后，params的参数会通过LinkedME服务器透传给app，由app根据参数进行相关跳转
      // 例如：详情页面的参数，写入到params中，这样在唤起app并获取参数后app根据参数跳转到详情页面
      var value1 = 1;
      var value2 = 2;
      data.params = '{"key1":"'+value1+'","key2":"'+value2+'"}'; //注意单引号和双引号的位置

    linkedme.link(data, function(err, response){
        if(err){
          // 生成深度链接失败，返回错误对象err
        } else {
          /* 
            生成深度链接成功，深度链接可以通过data.url得到，
            将深度链接绑定到<a>标签，这样当用户点击这
            个深度链接，如果是在pc上，那么跳转到深度链接二维
            码页面，用户用手机扫描该二维码就会打开app；如果
            在移动端，深度链接直接会根据手机设备类型打开ios
            或者安卓app 
           */
          document.body.innerHTML += '<a class="linkedme" href="'+response.url+'">LinkedME深度链接</a>'
        }
      },false);
    </script>
  </head>
  <body>
  </body>
</html>
```

> 提示：请使用`<a/>`标签作为打开app的跳转按钮，同时为了在Chrome及QQ中获得更好的用户体验（直接唤起app），请在a标签中添加class="linkedme"，并且为a标签的href属性设置值为生成的深度链接。示例：
>
> ```text
> <a href="https://lkme.cc/AfC/CeG9o5VH8" class="linkedme">打开应用</a>
> ```

注意：在iOS微信中必须手动触发深度链接，不能自动跳转。

提示：一个页面包含多个深度链接时，请添加QQ群：639389757沟通。

## 其他功能

### 测试模式

若想测试集成SDK后是否能正确生成深度链接，可以使用测试模式。测试模式需要设置 `data.type = "test";` 测试模式产生的数据将进入测试系统（Test）中。

注意：上线后务必设置 `data.type = "live"` ; 否则将影响APP线上数据的查看。
