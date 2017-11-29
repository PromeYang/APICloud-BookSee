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

## 常用的正则表达式

```
信用卡
[0-9]{13,16}
银联卡
^62[0-5]\d{13,16}$
Visa
^4[0-9]{12}(?:[0-9]{3})?$
万事达：
^5[1-5][0-9]{14}$
QQ号码：
[1-9][0-9]{4,14}
手机号码：
^(13[0-9]|14[5|7]|15[0|1|2|3|5|6|7|8|9]|18[0|1|2|3|5|6|7|8|9])\d{8}$
身份证：
^([0-9]){7,18}(x|X)?$
密码：
^[a-zA-Z]\w{5,17}$ 字母开头，长度在6~18之间，只能包含字母、数字和下划线
强密码：
^(?=.\d)(?=.[a-z])(?=.*[A-Z]).{8,10}$ 包含大小写字母和数字的组合，不能使用特殊字符，长度在8-10之间
7个汉字或14个字符：
^[\u4e00-\u9fa5]{1,7}$|^[\dA-Za-z_]{1,14}$
```

## 校验机动车识别代码(Vin)

```
// checkVin.js
// desc: 检验车辆识别代码(VIN)
// auth: promeyang
// time: 2017-11-09 13:35:33
// test: JF1SH52F6AG152654
// use: checkVin('JF1SH52F6AG152654')

function checkVin(code) {
    if (!code || code.length !== 17) return false;
    if (/[IOQ]/i.test(code)) return false;
    // 转换大写, 并生成字符数组, 等待计算
    var list = code.toUpperCase().split('');
    // VIN码对照值
    var matchCode = {
        "0": 0,
        "1": 1,
        "2": 2,
        "3": 3,
        "4": 4,
        "5": 5,
        "6": 6,
        "7": 7,
        "8": 8,
        "9": 9,
        "A": 1,
        "B": 2,
        "C": 3,
        "D": 4,
        "E": 5,
        "F": 6,
        "G": 7,
        "H": 8,
        "J": 1,
        "K": 2,
        "L": 3,
        "M": 4,
        "N": 5,
        "P": 7,
        "R": 9,
        "S": 2,
        "T": 3,
        "U": 4,
        "V": 5,
        "W": 6,
        "X": 7,
        "Y": 8,
        "Z": 9
    };
    // VIN码加权值
    var matchValue = [8, 7, 6, 5, 4, 3, 2, 10, 0, 9, 8, 7, 6, 5, 4, 3, 2];
    // 校验值
    var checkCode = list[8];
    // 加权总和
    var countCode = 0;
    // 结果值
    var resultCode = null;
    // 计算加权总和
    for (var i = 0; i < list.length; i++) {
        var _code = list[i];
        // console.log(_code, matchCode[_code], matchValue[i], matchCode[_code] * matchValue[i]);
        countCode += matchCode[_code] * matchValue[i];
    }
    // console.log(checkCode, countCode, countCode % 11);
    if (countCode % 11 === 10) resultCode = 'X';
    else resultCode = '' + (countCode % 11);
    // 返回校验结果
    return checkCode === resultCode;
}
```

## 常见问题

* 待续... 



