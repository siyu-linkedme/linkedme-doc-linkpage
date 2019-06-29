# 一键拉新

## 应用场景：

电子邮件/短信营销活动、网络搜索/官方网站、社交分享

## 场景重现：

1、电子邮件/短信营销活动：Lisa是某电商网站的会员，一天，她收到该网站发来的促销邮件/短信，“亲爱的Lisa，您购物车的商品正在降价，详情点击:http://www.xxx.com”。点击链接后，看到“新用户下载APP，将无论订单金额，免费配送”，Lisa点击了“立即下载”按钮， 进行下载，安装后打开，直接进入商品详情页。

2、网络搜索/官方网站：最近，闺蜜给小李推荐了一部好看的电影，她手机中的视频APP里没有版权，于是她选择到网络上直接搜索影片的名称观看。进入网站后，发现新用户下载APP，可以领取观影券，点击“立即领取”，开始下载APP，安装后打开，直接跳到影片详情页。

3、小赵在某直播App上观看直播，此时他将这个直播间通过QQ/微信等分享给小王，小王打开分享链接，进入H5页面，浏览片刻后，深深的被主播吸引，渴望交流一番，点击H5页面的“关注”按钮，自动下载APP，安装后打开，直接跳到直播间，继续观看。

## 优势

1、提高营销效率，降低拉新成本  
 2、提升用户体验，增加产品友好度

## 实现方式：

1、网页集成JS SDK，JS SDK创建深度链接，并向服务器传递参数，代码如下：

```text
//页面ID 
pageID = 36; 
//自定义iOS平台下App的下载地址
 ios_custom_url = "xxx";
 //自定义安卓平台下App的下载地址 
android_custom_url = "xxx";
```

JS SDK传参代码参见链接：[点击这里](https://pagedoc.lkme.cc/linkpage/linkpage-integration/js-sdk.html#%E5%88%9B%E5%BB%BA%E6%B7%B1%E5%BA%A6%E9%93%BE%E6%8E%A5)

2、客户端集成的 iOS / Android SDK负责解析服务器回传的深度链接参数，将页面展示给用户。代码如下:

```text
pageID = 36;（页面ID）
//获知用户打开APP前浏览的页面ID，可根据此ID将页面在APP内调出，展示给用户。
```

Android SDK 解析深度链接代码参见链接：[点击这里](https://pagedoc.lkme.cc/linkpage/linkpage-integration/android-sdk.html#%E8%A7%A3%E6%9E%90%E6%B7%B1%E5%BA%A6%E9%93%BE%E6%8E%A5%E5%8F%82%E6%95%B0%E5%B9%B6%E8%B7%B3%E8%BD%AC)   
IOS SDK 解析深度链接代码参见链接：[点击这里](https://pagedoc.lkme.cc/linkpage/linkpage-integration/ios-sdk.html#%E8%A7%A3%E6%9E%90%E6%B7%B1%E5%BA%A6%E9%93%BE%E6%8E%A5)

