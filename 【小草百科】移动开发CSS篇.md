# 【小草百科】移动开发CSS篇

## ios系统中元素被触摸时产生的半透明灰色遮罩怎么去掉

ios用户点击一个链接，会出现一个半透明灰色遮罩, 如果想要禁用，可设置`-webkit-tap-highlight-color`的`alpha`值为`0`，也就是属性值的最后一位设置为0就可以去除半透明灰色

```
a,button,input,textarea{-webkit-tap-highlight-color: rgba(0,0,0,0;)}
```

## 部分android系统中元素被点击时产生的边框怎么去掉

android用户点击一个链接，会出现一个边框或者半透明灰色遮罩, 不同生产商定义出来额效果不一样，可设置`-webkit-tap-highlight-color`的`alpha`值为`0`去除部分机器自带的效果

```
a,
button,
input,
textarea {
    -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
    -webkit-user-modify: read-write-plaintext-only;
}
```

-webkit-user-modify有个副作用，就是输入法不再能够输入多个字符

另外，有些机型去除不了，如小米2

对于按钮类还有个办法，不使用a或者input标签，直接用div标签

## webkit表单元素的默认外观怎么重置

```
.css{-webkit-appearance:none;}
```

## webkit表单输入框placeholder的颜色值能改变么

```
input::-webkit-input-placeholder{color:#AAAAAA;}
input:focus::-webkit-input-placeholder{color:#EEEEEE;}
```

## webkit表单输入框placeholder的文字能换行么

ios可以，android不行~

## 禁止ios 长按时不触发系统的菜单，禁止ios&android长按时下载图片

```
.css{-webkit-touch-callout: none}
```

## 禁止ios和android用户选中文字

```
.css{-webkit-user-select:none}
```

## 模拟按钮hover效果

移动端触摸按钮的效果，可明示用户有些事情正要发生，是一个比较好体验，但是移动设备中并没有鼠标指针，使用css的hover并不能满足我们的需求

```
.btn-blue:active{background-color: #357AE8;}
```

兼容性ios5+、部分android 4+、winphone 8

还可以使用官方的 `tapmode` 方式实现

```
<div id="leftBtn" class="ui-header__leftBtn" onclick="btnTap(1);" tapmode="active"></div>
```

## 屏幕旋转的样式

```
//竖屏时使用的样式
@media all and (orientation:portrait) {
    .css {}
}
//横屏时使用的样式
@media all and (orientation:landscape) {
    .css {}
}
```

## 开启硬件加速, 解决页面闪白, 保证动画流畅

```
.css {
    -webkit-transform: translate3d(0, 0, 0);
    -moz-transform: translate3d(0, 0, 0);
    -ms-transform: translate3d(0, 0, 0);
    transform: translate3d(0, 0, 0);
}
```

## android 上去掉语音输入按钮

```
input::-webkit-input-speech-button {display: none}
```

## flex布局

flex布局目前可使用在移动中，并非所有的语法都全兼容

兼容性：ios 4+、android 2.3+、winphone8+

```
// flex：定义布局为盒模型
.flex {
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
}
// flex-v：盒模型垂直布局
.flex-v {
    -webkit-box-orient: vertical;
    -webkit-flex-direction: column;
    -ms-flex-direction: column;
    flex-direction: column;
}
// flex-1：子元素占据剩余的空间
.flex-1 {
    -webkit-box-flex: 1;
    -webkit-flex: 1;
    -ms-flex: 1;
    flex: 1;
}
// flex-align-center：子元素垂直居中
.flex-align-center {
    -webkit-box-align: center;
    -webkit-align-items: center;
    -ms-flex-align: center;
    align-items: center;
}
// flex-pack-center：子元素水平居中
.flex-pack-center {
    -webkit-box-pack: center;
    -webkit-justify-content: center;
    -ms-flex-pack: center;
    justify-content: center;
}
// flex-pack-justify：子元素两端对齐
.flex-pack-justify {
    -webkit-box-pack: justify;
    -webkit-justify-content: space-between;
    -ms-flex-pack: justify;
    justify-content: space-between;
}
```

