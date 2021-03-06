# 免打包跨渠道推广

## 优势

1、精准评估渠道传播效果和用户质量。   
2、简化推广流程，提高推广效率。   
3、无需开发人员耗费时间，制作大量渠道包，节省开发成本。   
4、用户不必填写地推码，提高用户体验和转化率。

## 实际应用

1、地推数据高效统计：根据地推人员的“姓名+编号（防止重名）”创建深度链接，生成二维码，分发到地推人员手中，用户扫码后，无需填写地推码，因为深度链接可以按照参数区分用户来源。   


2、CPS渠道付费统计：依照“渠道名称”创建深度链接，分发给不同的渠道，用于标识用户来源。同时，APP结合自定义效果点上传付费效果点数据，后台根据用户来源统计不同渠道的收入，APP开发者可以按照投放效果，灵活付费。

## 实现方式

1、JS SDK创建深度链接时，在链接中添加自定义参数，示例如下：

```
//渠道名称   
channel = "地推"; 
//姓名
name = "张彭3";
//地域
area = "西二旗"；
```

JS SDK传参代码参见链接：[点击这里](https://pagedoc.lkme.cc/linkpage/linkpage-integration/js-sdk.html#%E5%88%9B%E5%BB%BA%E6%B7%B1%E5%BA%A6%E9%93%BE%E6%8E%A5) 

2、APP被唤醒后，服务器向Android / IOS SDK回传创建链接时生成的参数值并记录新增用户来源，示例如下：

```text
channel = "地推";（渠道名称）
name = "张彭3";（姓名）
area = "西二旗";（地域）
```

Android SDK 解析深度链接代码参见链接：[点击这里](https://pagedoc.lkme.cc/linkpage/linkpage-integration/android-sdk.html#%E8%A7%A3%E6%9E%90%E6%B7%B1%E5%BA%A6%E9%93%BE%E6%8E%A5%E5%8F%82%E6%95%B0%E5%B9%B6%E8%B7%B3%E8%BD%AC)   
IOS SDK 解析深度链接代码参见链接：[点击这里](https://pagedoc.lkme.cc/linkpage/linkpage-integration/ios-sdk.html#%E8%A7%A3%E6%9E%90%E6%B7%B1%E5%BA%A6%E9%93%BE%E6%8E%A5)

3、APP开发者根据自定义效果点进行埋点，并上报给LinkedMe服务器。

举个栗子：

1、某电商APP在渠道推广中，关注的核心指标是订单成交量，那么就可以在用户”完成支付“时设置埋点，支付成功后，将结果上报给LinkedMe服务器，订单量+1，服务端统计不同渠道产生的订单数量，帮助开发者分析渠道的优劣，及后续推广方案的调整。

```text
.track ( '支付成功页', {
    '商品名称' : 'XX硬盘',
    '商品价格' :' 599.00',
    '订单号' : 473894734895798,
    '订单状态' : '待发货'});  
```

2、某游戏APP在广告投放中，根据CPS结算，通过代码埋点的方式获取用户在游戏中首充金额，并将结果上报给LinkedMe服务器，服务端负责识别用户所属渠道，统计收入，为广告主提供可靠的结算依据。

```text
.track ( '充值完成页', {
'游戏ID' : '桃花洞主'，
'购买物品' : '流光桃花剑',
    '物品价格' : '68.00'
    });  
```

