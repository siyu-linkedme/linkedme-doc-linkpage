# 注册并开始

## 注册账号

### 注册账号

LinkedME账号通用于我们提供的所有产品，登录官方网站进行[注册](https://www.linkedme.cc/dashboard/index.html#/access/signup)。

### 开通使用

注册成功之后，即可[登录](https://www.linkedme.cc/dashboard/index.html#/access/signin)使用。

## 创建应用

### 创建应用

成功登录后台之后，在“应用中心”中创建新应用。

![](.gitbook/assets/2.2.1-1.jpg)

输入APP名字之后，上传LOGO，点击“创建”。

![](.gitbook/assets/2.2.1-2.jpg)

创建完成后，根据集成引导完成SDK的集成。

## 应用设置

### 应用设置

点击“设置”，可以对应用进行设置。

![](.gitbook/assets/2.3.1.jpg)

1. 点击修改应用名字
2. 点击修改应用Logo
3. 点击删除应用

注意事项：如果误删应用后，请发邮件到support@linkedme.cc。

### 链接设置

为了实现不同平台的精准跳转，需要针对不同平台进行对应的链接配置。

**有iOS应用**

![](.gitbook/assets/2.3.2.1-1.jpg)

1. 勾选“是否有iOS应用”项。
2. URI Scheme，点击输入框输入您的iOS应用的URI Scheme协议，示例：linkedmedemo
3. 下载地址，可以是AppStore上的下载地址，也可以自定义
4. Universal Links为iOS官方深度链接标准，iOS 9.0以上系统支持，可实现应用间无缝跳转。访问[苹果开发者网站](https://developer.apple.com/)，在Appid中查看Bundle ID和Apple App Prefix的值

**无iOS应用**

![](.gitbook/assets/2.3.2.1-2.jpg)

1. 不用勾选“是否有iOS应用”项。
2. iOS URL，苹果设备访问该深度链接所跳转到的地址

**有Android应用**

![](.gitbook/assets/2.3.2.2-1.jpg)

1. 勾选“是否有Android应用”项
2. URI Scheme，点击输入框输入您的Android应用的URI Scheme协议，示例：linkedmedemo
3. Package Name，为Android应用的包名
4. 下载地址，可以是应用商店的下载地址，也可以自定义
5. App Links是谷歌官方应用深度链接标准，Android 6.0以上版本支持，可以实现应用间无缝跳转。开启方法：使用java的 keytool 命令获取SHA256指纹证书：`keytool -list -v -keystore my-release-key.keystore`，请填写线上发布包所对应的证书”

**无Android应用**

![](.gitbook/assets/2.3.2.2-2.jpg)

1. 不用勾选“是否有Android”应用
2. Android URL，Android设备访问该深度链接所跳转到的地址

**PC端设置**

如果是通过PC端访问该深度链接，可以有两种跳转方式。

![](.gitbook/assets/2.3.2.2-3.jpg)

1. 默认值，跳转到二维码页面，手机扫描之后跳转到APP内对应页面
2. 自定义，可以自定义为您所希望的跳转地址，建议配置为官网地址

**高级设置**

当app没有安装时，点击深度链接后跳转到的下载中转页

![](.gitbook/assets/gaoji.png)

