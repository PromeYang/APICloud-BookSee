# 【小草百科】移动开发JS篇

## 移动端click屏幕产生200-300ms的延迟响应

移动设备上的web网页是有300ms延迟的，往往会造成按钮点击延迟甚至是点击失效。

### 解决方案

* fastclick可以解决在手机上点击事件的300ms延迟
* zepto的touch模块，tap事件也是为了解决在click的延迟问题
* hammer的tap事件
* APICloud官方自带的tapmode

## 移动端touch事件(区分webkit 和 winphone)

当用户手指放在移动设备在屏幕上滑动会触发的touch事件, 触摸事件的响应顺序

```
ontouchstart --> ontouchmove --> ontouchend --> onclick
```

### 以下支持webkit

* touchstart——当手指触碰屏幕时候发生。不管当前有多少只手指
* touchmove——当手指在屏幕上滑动时连续触发。通常我们再滑屏页面，会调用event的preventDefault()可以阻止默认情况的发生：阻止页面滚动
* touchend——当手指离开屏幕时触发
* touchcancel——系统停止跟踪触摸时候会触发。例如在触摸过程中突然页面alert()一个提示框，此时会触发该事件，这个事件比较少用

### 以下支持winphone

* MSPointerDown——当手指触碰屏幕时候发生。不管当前有多少只手指
* MSPointerMove——当手指在屏幕上滑动时连续触发。通常我们再滑屏页面，会调用css的html{-ms-touch-action: none;}可以阻止默认情况的发生：阻止页面滚动
* MSPointerUp——当手指离开屏幕时触发

## 屏幕旋转的事件

window.orientation，取值：正负90表示横屏模式、0和180表现为竖屏模式；

```
window.onorientationchange = function() {   
    switch (window.orientation) {       
        case -90: 
        case 90:
            alert("横屏:" + window.orientation);       
        case 0:
        case 180:
            alert("竖屏:" + window.orientation);       
            break;   
    } 
} 
```

## 常见问题

* 待续... 



