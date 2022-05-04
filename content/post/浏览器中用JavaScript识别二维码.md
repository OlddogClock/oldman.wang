---
title: "浏览器中用JavaScript识别二维码"
date: 2022-04-27T16:17:04+08:00
draft: false
categories:
  - 程序员杂志
tags:
  - JavaScript
  - 二维码
---
浏览器中使用纯JS读取识别二维码内容。可用于上传二维码图时，对二维码内容进行校验，避免上传非法二维码。
<!--more-->
使用jsQR对二维码进行识别 https://github.com/cozmo/jsQR ，为了更加方便的嗲用，对浏览器中的[jsQR](https://github.com/cozmo/jsQR/blob/master/dist/jsQR.js)稍微有些改动：
* 可以传入图片URL、Base64、Blob
* 不用传入图片尺寸
* 用Promise方式

```js
// 原调用方式需要传入Uint8ClampedArray类型的图片数据，并要知道图片的宽高
const code = jsQR(imageData, width, height, options?);

// 修改后直接传入图片URL、Base64、Blob
jsQR('https://xxx/ooo.jpg').then((success) => {
  console.log(success)
}, () => {
  console.log('error')
})

```

如果用在jQuery Uploader中，可以实现上传前检查二维码内容，比如：
```js
$('#upload_alipay').fileupload({
  add: function(e, data){
    jsQR(URL.createObjectURL(data.files[0])).then((success) => {
      // 只允许支付宝二维码
      if (!/^https:\/\/qr\.alipay\.com\//.test(success.data)) {
        alert('请上传正确的支付宝收款二维码')
      }else{
        data.submit()
      }
    }, () => {
      alert('请上传正确的支付宝收款二维码')
    })
  }
})
```

对jsQR的改动如下：

```js
// jsQR function 改名为 _jsQR
function _jsQR(data, width, height, providedOptions) {
    if (providedOptions === void 0) { providedOptions = {}; }
    var options = defaultOptions;
    Object.keys(options || {}).forEach(function (opt) {
        options[opt] = providedOptions[opt] || options[opt];
    });
    var shouldInvert = options.inversionAttempts === "attemptBoth" || options.inversionAttempts === "invertFirst";
    var tryInvertedFirst = options.inversionAttempts === "onlyInvert" || options.inversionAttempts === "invertFirst";
    var _a = binarizer_1.binarize(data, width, height, shouldInvert), binarized = _a.binarized, inverted = _a.inverted;
    var result = scan(tryInvertedFirst ? inverted : binarized);
    if (!result && (options.inversionAttempts === "attemptBoth" || options.inversionAttempts === "invertFirst")) {
        result = scan(tryInvertedFirst ? binarized : inverted);
    }
    return result;
}

// 用canvas读取并转化图片为Uint8ClampedArray类型
function jsQR(data, providedOptions){
    var width
    var height
    var canvas = document.createElement('canvas')
    var ctx = canvas.getContext('2d')
    var image = new Image()
    image.crossOrigin = 'anonymous'
    image.src = data
    return new Promise(function (resolve, reject) {
        image.onload = () => {
            width = image.width
            height = image.height
            canvas.width = width
            canvas.height = height
            ctx.drawImage(image, 0, 0, width, height)
            var imageData = ctx.getImageData(0, 0, width, height)
            var result = _jsQR(imageData.data, width, height, providedOptions)
            if(result){
                resolve(result)
            }else{
                reject()
            }
        }
    })
}
jsQR.default = jsQR;
exports.default = jsQR;

```