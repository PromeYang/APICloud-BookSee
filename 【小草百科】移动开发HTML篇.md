# 【小草百科】移动开发HTML篇

## meta基础知识

* H5页面窗口自动调整到设备宽度，并禁止用户缩放页面

```
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />
```

* 图片离线上传功能, 图片缓存策略, 图片预览
* 本地图片如何展示在 `<img>`和 `"background-image"` 中
* 大型数据的本地存储与读写, 因为 `localStorage` 有大小上限, 大概在10M以内, 具体数额不明
* 待续... 

## 模块依赖

* fs - 必选, 用于文件操作, 是实现离线存储的核心模块
* trans - 必选, 用于转换base64数据格式的图片, 可以全兼容所有平台的
* imageFilter - 必选, 用于获取图片信息以及压缩图片大小
`<img>`和 `"background-image"` 展示
* UIMediaScanner - 可选, 多图选择时会引入
* imageBrowser - 可选, 简单的相册展示

## 案例分析

**P.S. 比较长, 干货可以跳过**

* 接到一个项目的需求是要做离线的图片存储, 在没有网络的情况下, 需要把拍照的或者选择的多张图片存储在手机中, 可以随意新增或者删除已选的图片
* 图片选择一般需要拍照, 我使用 `api.getPicture` 来实现, 这个方法能给我提供两个返回值, 一个是图片的本地路径, 一个是base64, 当时想直接展示这个本地路径, 发现不行, base64可行
* 图片多选我使用 `UIMediaScanner`, 返回的值是在不同平台下, 是一个不同的值, 是一个相对路径, 本地路径需要用模块中的 `transPath` 方法拿到, 在iOS中, 拿到的相对路径是可以展示在html中, 但是 `imageBrowser` 不支持相对路径的展示
* 为了能让图片在不同的系统上, 不用做兼容处理就能通用的在html中展示, 最后发现 `base64` 是最简单最理想的实现方式了, 上传的时候可以直接采用base64格式的数据上传, 只需要后台配合一下就好
* 由于我要做相册的展示, 其实就是看大图的功能, 我使用到了 `imageBrowser`, 它需要接受绝对路径来展示, 不管是本地的, 还是网络的
* 而 `api.getPicture` 和 `UIMediaScanner` 拿到的文件都是在 `cache` 路径的, 也就是会面临着被清除的可能, 所以必须调用 `fs` 来把这些文件真正的备份到 `fs://` 目录下
* 一开始做 `base64` 的时候, 其实是考虑使用 `localStorage` 的, 毕竟官方已经封装了, 使用比较方便, 平时一些小数据的存储都是用这个, 但是后来发现存了几张图片之后, 就触发了一个报错 `QuotaExceededError(DOM Exception 22): The quota has been exceeded`, 其实就是超出了 `localStorage` 存储的上限了, 这条路子走不通, 所以最后只能想到用 `fs` 来做文件存储了
* 最后用一个文件来存储图片的信息, 文件的内容是一个 `JSON.stringify` 之后的对象 `photoData` , 有三个 `key` 值
* `photoData.uid` 用一个 `new Date().getTime()` 来标识唯一的文件
* `photoData.fsPath` 标识着这个文件对应的本地路径
* `photoData.base64` 存储着真正的图片数据, 用于html展示
* 至此, 只要维护好 `photoData` 文件 和 对应 `photoData.fsPath` 的本地文件, 就可以做到在任意场景中使用了
* 这个案例核心在于解决离线存储图片, 以及各种展示的需要, 同理在对于一些大数据的存储上面也可以参照这个思路, 见参考代码

## 解决方案

* 使用 `fs` 模块提供的 `sync` 同步方法, 替代 `localStorage` 来进行文件管理以及大数据管理
* 使用 `trans` 和 `imageFilter` 来实现图片压缩和图片转换 `base64` , 用于后续的在html中的图片展示和文件上传

## 代码参考

### 本地文件操作封装Sync版

提供以下方法

* setFile
* getFile
* rmFile

#### setFile(opts, callback)

```
setFile: function(opts, callback) {
    var ret, fs = api.require('fs');
    var fd = null;
    opts.path = 'fs://data/' + opts.path;
    ret = fs.existSync({
        path: opts.path
    });
    if (!ret.exist) {
        ret = fs.createFileSync({
            path: opts.path
        });
    }
    ret = fs.openSync({
        path: opts.path,
        flags: 'read_write'
    });
    if (ret.status) {
        fd = ret.fd
        ret = fs.writeSync({
            fd: fd,
            data: JSON.stringify(opts.content),
            offset: 0,
            overwrite: true
        });
        if (ret.status) {
            ret = fs.closeSync({
                fd: fd
            });
            if (ret.status) {
                callback && callback(ret);
            } else {
                alert(JSON.stringify(ret));
            }
        } else {
            alert(JSON.stringify(ret));
        }
    } else {
        alert(JSON.stringify(ret));
    }
}
```

#### getFile

```
getFile: function(opts) {
    var ret, fs = api.require('fs');
    var fd = null;
    opts.path = 'fs://data/' + opts.path;
    ret = fs.existSync({
        path: opts.path
    });
    if (!ret.exist) {
        return undefined;
    }
    ret = fs.openSync({
        path: opts.path,
        flags: 'read_write'
    });
    if (ret.status) {
        fd = ret.fd
        ret = fs.readSync({
            fd: fd
        });
        if (ret.status) {
            var result = JSON.parse(ret.data);
            ret = fs.closeSync({
                fd: fd
            });
            if (ret.status) {
                return result;
            } else {
                alert(JSON.stringify(ret));
            }
        } else {
            alert(JSON.stringify(ret));
        }
    } else {
        alert(JSON.stringify(ret));
    }
}
```

#### rmFile

```
rmFile: function(opts, callback) {
    var ret, fs = api.require('fs');
    var fd = null;
    if (!opts.normal) opts.path = 'fs://data/' + opts.path;
    ret = fs.existSync({
        path: opts.path
    });
    if (ret.exist) {
        ret = fs.removeSync({
            path: opts.path
        });
        if (ret.status) {
            callback && callback(ret);
        } else {
            alert(JSON.stringify(ret));
        }
    }
}
```

### UIMediaScanner多图选择之后存储到本地

```
function UIMediaScannerAction(list) {
    _.each(list, function(item, index) {
        (function(item, index) {
            UIMediaScanner.transPath({
                path: item.path
            }, function(ret, err) {
                if (ret) {
                    var path = ret.path;
                    var imgName = new Date().getTime() + '.jpg';
                    var imageFilter = api.require('imageFilter');
                    imageFilter.compress({
                        img: path,
                        size: {
                            w: 640,
                            h: 1136
                        },
                        quality: 0.05,
                        save: {
                            imgPath: 'fs://tmp',
                            imgName: imgName
                        }
                    }, function(ret, err) {
                        if (ret.status) {
                            var trans = api.require('trans');
                            trans.decodeImgToBase64({
                                imgPath: 'fs://tmp/' + imgName
                            }, function(ret, err) {
                                if (ret && ret.status) {
                                    var base64 = 'data:image/jpg;base64,' + ret.base64Str;
                                    var fs = api.require('fs');
                                    var ret = fs.removeSync({
                                        path: 'fs://tmp/' + imgName
                                    });
                                    // alert(base64.length);
                                    // return;
                                    // alert(base64.slice(0, 100));return;
                                    storePhoto(path, base64);
                                } else {
                                    alert(JSON.stringify(err));
                                }
                            });
                        } else {
                            alert(JSON.stringify(err));
                        }
                    });
                } else {
                    alert(JSON.stringify(err));
                }
            });
        })(item, index);
    });
}
```

### 图片存储到本地

```
function storePhoto(path, base64) {
    var filename = (function() {
        var array = path.split('/');
        return array[array.length - 1];
    })();
    var photoData = {};
    photoData.uid = new Date().getTime() + '';
    photoData.base64 = base64;
    var fs = api.require('fs');
    var newPath = 'fs://photos';
    fs.copyTo({
        oldPath: path,
        newPath: newPath
    }, function(ret, err) {
        if (ret.status) {
            var ret = fs.renameSync({
                oldPath: newPath + '/' + filename,
                newPath: newPath + '/' + photoData.uid + '.jpg'
            });
            photoData.fsPath = newPath + '/' + photoData.uid + '.jpg';
            var key = 'photoData' + photoData.uid;
            _g.setFile({
                path: key,
                content: photoData
            });
            main.list.push(key);
            transPhotos.push(photoData.fsPath);
            photoData = null;
            updatePhotos();
        } else {
            alert(JSON.stringify(err));
        }
    });
}
```

