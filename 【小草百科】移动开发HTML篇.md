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

## 


