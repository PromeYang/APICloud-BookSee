# 【小草百科】移动开发HTML篇

## 基础页面模板

```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no" name="viewport">
    <meta content="telephone=no" name="format-detection">
    <meta content="email=no" name="format-detection">
    <title>标题</title>
    <link rel="stylesheet" href="index.css">
</head>
<body>
    这里开始内容
    <script src="index.js"></script>
    <script></script>
</body>
</html>
```

## meta基础知识

* 设置字符集

```
<meta charset="UTF-8">
```

* H5页面窗口自动调整到设备宽度，并禁止用户缩放页面

```
<meta name="viewport" content="maximum-scale=1.0,minimum-scale=1.0,user-scalable=0,width=device-width,initial-scale=1.0"/>
```

* 忽略将页面中的数字识别为电话号码

```
<meta content="telephone=no" name="format-detection">
```

* 忽略对邮箱地址的识别

```
<meta content="email=no" name="format-detection">
```

* winphone系统a、input标签被点击时产生的半透明灰色背景怎么去掉

```
<meta name="msapplication-tap-highlight" content="no">
```

* 待续... 

## 打电话发短信的怎么实现

```
打电话
<a href="tel:0755-10086">打电话给:0755-10086</a>
发短信，winphone系统无效
<a href="sms:10086">发短信给: 10086</a>
```

## 取消input在ios下，输入的时候英文首字母的默认大写

```
<input autocapitalize="off" autocorrect="off" />
```

## 实现键盘的搜索按钮

```
<form onsubmit="return false;">
	<input type="search">
</form>
```

## 性能优化方案

发布前必要检查项

* 所有图片必须有进行过压缩
* 考虑适度的有损压缩，如转化为80%质量的jpg图片
* 考虑把大图切成多张小图，常见在banner图过大的场景

加载性能优化, 达到打开足够快

* 数据离线化，考虑将数据缓存在 localStorage
* 初始请求资源数 < 4 注意！
* 图片使用CSS Sprites 或 DataURI
* 外链 CSS 中避免 @import 引入
* 考虑内嵌小型的静态资源内容
* 初始请求资源gzip后总体积 < 50kb
* 静态资源(HTML/CSS/JS/Image)是否优化压缩？
* 避免打包大型类库
* 确保接入层已开启Gzip压缩（考虑提升Gzip级别，使用CPU开销换取加载时间） 注意！
* 尽量使用CSS3代替图片
* 初始首屏之外的静态资源（JS/CSS）延迟加载 注意！
* 初始首屏之外的图片资源按需加载（判断可视区域） 注意！
* 单页面应用(SPA)考虑延迟加载非首屏业务模块
* 开启Keep-Alive链路复用

运行性能优化, 达到操作足够流畅

* 避免 iOS 300+ms 点击延时问题 注意！
* 缓存 DOM 选择与计算
* 避免触发页面重绘的操作
* Debounce连续触发的事件(scroll / resize / touchmove等)，避免高频繁触发执行 注意！
* 尽可能使用事件代理，避免批量绑定事件
* 使用CSS3动画代替JS动画
* 避免在低端机上使用大量CSS3渐变阴影效果，可考虑降级效果来提升流畅度
* HTML结构层级保持足够简单
* 尽能少的使用CSS高级选择器与通配选择器
* Keep it simple

## ios纯数字键盘

```
<input type="number" pattern="[0-9]*">
```


