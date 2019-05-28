---
description: 更多应用场景：免填邀请码绑定关系、场景还原、自动加好友...
---

# 免填邀请码

### 功能

深度链接可以直接携带信息，无需用户手动填写邀请码。

### 作用：

新用户点击深度链接下载安装后，打开APP跳转到分享页；  
新用户注册APP时，自动获取邀请者用户昵称，绑定邀请者关系。  
新用户注册APP时，自动获取邀请者用户昵称，添加好友关系。   
\*注意：用户昵称只是个栗子，可替换成APP内识别用户的其他唯一标识，比如QQ可替换成QQ号，抖音可替换成抖音号。

### 行业分类：

1、电商类：分享商品给好友，请好友帮领优惠券、促销打折活动拉人；   
2、游戏类：组局玩游戏，邀请好友得装备，体力；   
3、新闻资讯：邀请好友得赏金，单纯的内容分享；

### 优势：

1、提升用户体验   
2、提升用户活跃度   
3、提高用户转化率   
4、提高用户邀请关系的准确率   
5、促进用户关系链建立   
6、提升开发效率   
7、提升APP推广裂变效率   
......

### 实现方式：

Step1：用户A在进行分享时，JS SDK创建深度链接，并向LinkedMe 服务器传递必要参数，示例如下：

```text
//页面ID
pageID = 36;
//详情页类型
PageType = goods；
//用户昵称
UserName=小栗鸭；
```

JS SDK传参代码参见链接：[https://pagedoc.lkme.cc/linkpage/linkpage-integration/js-sdk.html\#%E5%88%9B%E5%BB%BA%E6%B7%B1%E5%BA%A6%E9%93%BE%E6%8E%A5](https://pagedoc.lkme.cc/linkpage/linkpage-integration/js-sdk.html#%E5%88%9B%E5%BB%BA%E6%B7%B1%E5%BA%A6%E9%93%BE%E6%8E%A5)

Step2：用户B打开来自社交好友的分享页面，点击深度链接后，唤起APP（如用户未安装APP，会先跳转至应用商店或apk下载地址，进行下载，安装后打开APP），LinkedMe 服务器回传创建链接时，所传递的参数值给Android / IOS SDK，示例如下：

```
pageID = 36;（页面ID）
//获知用户打开APP前浏览的页面ID，可根据此ID将页面在APP内调出，展示给用户，以达到场景还原。
PageType = goods；（详情页类型）
UserName=小栗鸭；（用户昵称）
//获知邀请人的用户昵称，用户注册后自动绑定邀请关系，加好友。
```

Android SDK 解析深度链接代码参见链接：[https://pagedoc.lkme.cc/linkpage/linkpage-integration/androidsdk.html\#%E8%A7%A3%E6%9E%90%E6%B7%B1%E5%BA%A6%E9%93%BE%E6%8E%A5%E5%8F%82%E6%95%B0%E5%B9%B6%E8%B7%B3%E8%BD%AC](https://pagedoc.lkme.cc/linkpage/linkpage-integration/android-sdk.html#%E8%A7%A3%E6%9E%90%E6%B7%B1%E5%BA%A6%E9%93%BE%E6%8E%A5%E5%8F%82%E6%95%B0%E5%B9%B6%E8%B7%B3%E8%BD%AC)  
IOS SDK 解析深度链接代码参见链接：[https://pagedoc.lkme.cc/linkpage/linkpage-integration/iossdk.html\#%E8%A7%A3%E6%9E%90%E6%B7%B1%E5%BA%A6%E9%93%BE%E6%8E%A5](https://pagedoc.lkme.cc/linkpage/linkpage-integration/ios-sdk.html#%E8%A7%A3%E6%9E%90%E6%B7%B1%E5%BA%A6%E9%93%BE%E6%8E%A5)

