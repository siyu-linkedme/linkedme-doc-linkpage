# iOS 平台

## **Q1:  为什么点击按钮可以直接从微信唤起app？**

A1: 使用[Universal Link](https://www.linkedme.cc/blog/square/586f15db8ecaaf415cbcc6ff)技术，可以通过链接直接唤起app。

## **Q2:  为什么右上角会出现lkme.cc？**

A2: 因为使用了苹果的[Universal Link](https://www.linkedme.cc/blog/square/586f15db8ecaaf415cbcc6ff)技术打开App。

## **Q3:  可以取消显示么？**

A3: 不可以，因为这是系统级的，无法控制。

## **Q4:  如果用户没有安装app会怎么样？**

A4: 点击前往App store下载下载完成后，打开app会跳转到当前分享页面。

## **Q5:  如果没有点击前往App store下载自己去App store搜索下载，下载完成后可以跳转到分享页吗？**

A5: 不可以，只有点击前往App store下载按钮才会记录浏览器指纹信息，用于匹配跳转指定页，如果没有点击按钮就不会记录信息。

## **Q6:  无Android版本或未在腾讯应用宝上架，iOS应用未安装的情况下，在微信中是否可以打开App store前往下载？**

A6: 可以！请在JS SDK生成深度链接时中采用如下配置即可：

```text
1. 设置ios_custom_url=App Store下载地址；
2. 设置ios_direct_open=true；
```

