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
# 基本配置
## 初始化LinkedME全局对象
<font color="red">注意：请在服务器环境下测试，本地打开存在跨域问题</font>

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

