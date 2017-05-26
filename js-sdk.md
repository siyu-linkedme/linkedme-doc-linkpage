# 准备工作
## 获取LinkedME Key
<font color="red">新用户</font>：在官网网站[注册账号](https://www.linkedme.cc/dashboard/index.html#/access/signup)，注册后[创建应用](https://www.linkedme.cc/dashboard/index.html#/app/aplt/create)，在后台导航栏“设置”中查看LinkedME Key。
<font color="red">老用户</font>：已经在官网网站注册账号，直接[创建应用](https://www.linkedme.cc/dashboard/index.html#/app/aplt/create)（可以创建多个应用），直接到导航栏“设置”中查看LinkedME Key。

## 获取LinkedME Web SDK
在线链接：https://lkme.cc/js/linkedme.min.js


# 导入SDK
## 导入LinkedME Web SDK

在html文件的head标签里增加如下代码：

```js
  <!-- 加载js -->
  <script src="https://lkme.cc/js/linkedme.min.js" ></script>
```
> 提示：JS SDK会不定期优化跳转逻辑，请不要将JS SDK下载到本地加载。

# 基本配置
## 初始化LinkedME全局对象
<font color="red">注意：请在服务器环境下测试，本地打开存在跨域问题</font>。

```js	
/* 
  接口名称: init(linkedme_key, data, callback)
  参数说明：
    linkedme_key: 每个app会分配唯一一个linkedme key，用户在linkedme官网创建app之后可以在设置菜单里面找到linkedme_key 【必选】
    data: 初始化linkedme对象参数，比如测试时需要添加data.type="test",上线时需要修改为"live",如果传null,默认为"live" 【可选】
    callback: 回调函数 【可选】
*/
```

示例代码1：

```js
<script>
  linkedme.init("linkedme_key", null, null);
</script>
```

示例代码2：

```js
<script>
  var data = {};
  data.type = "live";  //表示现在使用线上模式,如果填写"test", 表示测试模式.
  linkedme.init("linkedme_key", data, function(err, data){
    if(err){
    // 初始化失败，返回错误对象err
    } else {
    // 初始化成功，可以不做处理
    }
  });
</script>
```


# 深度链接功能
本模块实现的功能是创建深度链接及通过深度链接跳转到APP内的详情页面，若想要使用如下功能，请务必实现“基本配置”中的内容

## 创建深度链接
通过js创建深度链接，例如在H5页面中通过js将该页面的深度链接写到“打开APP”按钮下

```js
/* 
  接口名称： link(data, callback, autoSelect)
  参数说明：
    data ：生成深度链接的请求参数【必选】
    callback ：回调函数 【必选】
    autoSelect ：生成深度链接之后是否自动打开深度链接，默认值为false 【可选】
*/
```

示例代码：

```js
<script>
  var data = {};
  data.type = "live";  //表示现在使用线上模式,如果填写"test", 表示测试模式.【可选】
  data.feature = "功能名称"; // 自定义深度链接功能，多个名称用逗号分隔，【可选】
  data.stage = "阶段名称"; // 自定义深度链接阶段，多个名称用逗号分隔，【可选】
  data.channel = "渠道名称"; // 自定义深度链接渠道，多个名称用逗号分隔，【可选】
  data.tags = "标签名称"; // 自定义深度链接标签，多个名称用逗号分隔，【可选】
  data.ios_custom_url = ""; // 自定义iOS平台下App的下载地址，如果是AppStore的下载地址可以不用填写，【可选】
  data.android_custom_url = "";// 自定义安卓平台下App的下载地址，【可选】
  // 下面是自定义深度链接参数，用户点击深度链接打开app之后，params的参数会通过LinkedME服务器透传给app，由app根据参数进行相关跳转
  // 例如：详情页面的参数，写入到params中，这样在唤起app并获取参数后app根据参数跳转到详情页面
  var value1 = 1;
  var value2 = 2;
  data.params = '{"key1":"'+value1+'","key2":"'+value2+'"}'; //注意单引号和双引号的位置
  linkedme.link(data, function(err, data) {
    if (err) {
      // 生成深度链接失败，返回错误对象err
    } else {
      // 生成深度链接成功，深度链接可以通过data.url得到
      data.url
    }
  },false);
</script>
```

##完整代码示例

```
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
      data.ios_custom_url = ""; // 自定义iOS平台下App的下载地址，如果是AppStore的下载地址可以不用填写，【可选】
      data.android_custom_url = "";// 自定义安卓平台下App的下载地址，【可选】
      // 下面是自定义深度链接参数，用户点击深度链接打开app之后，params的参数会通过LinkedME服务器透传给app，由app根据参数进行相关跳转
      // 例如：详情页面的参数，写入到params中，这样在唤起app并获取参数后app根据参数跳转到详情页面
      var value1 = 1;
      var value2 = 2;
      data.params = '{"key1":"'+value1+'","key2":"'+value2+'"}'; //注意单引号和双引号的位置

	linkedme.link(data, function(err, data){
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
          document.body.innerHTML += '<a class="linkedme" href="'+data.url+'">LinkedME深度链接</a>'
        }
      },false);
    </script>
  </head>
  <body>
  </body>
</html>
```


# 其他功能
## 测试模式
若想测试集成SDK后是否能正确生成深度链接，可以使用测试模式。测试模式需要设置 `data.type = "test"; ` 测试模式产生的数据将进入测试系统（Test）中。


注意：<font color="red">上线后务必设置  `data.type = "live"` ;   否则将影响APP线上数据的查看</font>。






