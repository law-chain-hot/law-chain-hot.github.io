---
title: LocalStorage 如何储存图片
date: 2021-03-10 11:34:22
description:
toc: true
tags:
category: technology
---

# 问题难点

- 难点1：Localstorage 只能存储字符串，不能储存二进制图片  
解决办法：储存 `base64` 编码的图片

- 难点2：如何解析 `base64` 编码的图片  
解决办法： `<img src="data:image/png;base64, (具体编码)" />`

```html
  <img src="data:image/png;base64, (具体编码)" />
```


# 解决思路
```js
// img -> convertImgToBase64(img) -> store base64

bannerImage = document.getElementById('bannerImg');
imgData = getBase64Image(bannerImage);
localStorage.setItem("imgData", imgData);
```

Here is the function that converts the image to a Base64 string:

```js
function getBase64Image(img) {
    var canvas = document.createElement("canvas");
    canvas.width = img.width;
    canvas.height = img.height;

    var ctx = canvas.getContext("2d");
    ctx.drawImage(img, 0, 0);

    var dataURL = canvas.toDataURL("image/png");

    return dataURL.replace(/^data:image\/(png|jpg);base64,/, "");
}
```

---
# 代码展示：例子
```html
<img src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAAUA
    AAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO
        9TXL0Y4OHwAAAABJRU5ErkJggg==" alt="Red dot" />
```
<img src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAAUA
    AAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO
        9TXL0Y4OHwAAAABJRU5ErkJggg==" alt="Red dot" />

👆 上面这个小红点便是上方代码的结果


# 参考资料
1. [html5本地存储图片方法](https://www.haorooms.com/post/html5_storageimage)
2. [How to save an image to localStorage and display it on the next page?](https://stackoverflow.com/questions/19183180/how-to-save-an-image-to-localstorage-and-display-it-on-the-next-page)