# 数据指标

## 基本使用

### 名词解释

LinkPage的数据指标主要包括从深度链接创建到最终用户访问到的这个流程中，深度链接被曝光、点击、打开、激活、扫码的次数，解释如下：

* 曝光：深度链接对应的H5页面的加载次数
* 点击：深度链接的点击量
* 打开：通过深度链接打开应用的次数
* 激活：通过点击深度链接下载并激活应用的数量
* 扫码：PC端二维码页被扫描的次数。在PC端访问深度链接，默认情况下，会跳转到二维码页

### 基本控件

![](../.gitbook/assets/5.1.2.jpg)

1. 时段控件：默认选中最近30天，页面上所展示的数据为选中时段内的数据
2. 应用中心：返回全部应用概览页面
3. 管理中心：账号及授权管理
4. 信息中心：提交用户反馈及Linkedme消息通知
5. 模式：点击Live查看线上数据，点击Test查看测试数据；线上数据和测试数据互不干扰
6. 切换应用

## 数据统计

### 概览

![](../.gitbook/assets/5.2.1.jpg)

概览里的数据展示的是在所选时段内活跃的深度链接和深度链接的各指标数据。同时也包括分平台的各指标数据。  
iOS/Android平台是只通过该平台对深度链接的访问行为，PC是指通过PC端对深度链接的访问，默认情况下PC端访问深度链接会跳转到二维码页面，所以PC端的数据还包括扫码的数据。

### 深度链接分析

![](../.gitbook/assets/5.2.2.1.jpg)

1. 按照来源筛选深度链接：Dashboard指通过后台手动添加的深度链接；SDK是指通过SDK自动创建的深度链接；Test是指当您通过“测试”菜单项进行测试时生成的深度链接
2. 按照渠道搜索深度链接
3. 按照标签搜索深度链接
4. 选择“深度链接”/“深度链接名称”来进行搜索
5. 手动创建深度链接时，可以选择创建单条或者批量创建。创建链接时可以使用APP的默认配置或者自定义配置，满足您自定义推广的需求。

![](../.gitbook/assets/5.2.2.2.jpg)

1. 对于每一条深度链接都可以通过“分析”查看详细数据，也可以通过“编辑”为深度链接命名或者标记属性等，同时可以删除不再关注的深度链接。
2. 当您通过左侧多选框选择多个深度链接时，可以通过这里分析汇总数据或删除多个链接
3. 表格中展示的字段可以通过这里来编辑内容及展示顺序

![](../.gitbook/assets/5.2.2.3.jpg)

1. 通过下方选中的字段可以通过这里拖动编辑展示顺序，排在第一位的字段在展示时会固定到首列，这样您分析数据时能保证通过首位的字段来区分不同的深度链接
2. 对于您上传的参数可以直接选中“参数”来查看完整的json形式数据，或者也可以添加自定义参数，例如参数是 {"$control":{"link":"topic://12345"}} ，您可以在自定义字段中添加 link 参数，这样就可以直接在页面上显示 topic://12345 添加的自定义字段可以从列表中删除

