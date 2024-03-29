### 二维码识别引发的相关经验积累，记录做二维码识别时遇到的所有问题

#### canvas
```js
画布可以对视频video进行截图

canvas = $('#canvas')
context = canvas.getContext("2d");

// 根据输入源进行画图
context.drawImage(image, dx, dy) 在画布指定位置绘制原图
context.drawImage(image, dx, dy, dw, dh) 在画布指定位置上按原图大小绘制指定大小的图
context.drawImage(image, sx, sy, sw, sh, dx, dy, dw, dh) 剪切图像，并在画布上定位被剪切的部分

image	规定要使用的图像、画布或视频
sx	可选。开始剪切图片的 x 坐标位置
sy	可选。开始剪切图片的 y 坐标位置
sw	可选。被剪切图像的宽度（就是裁剪之前的图片宽度，这里的宽度若小于图片的原宽。则图片多余部分被剪掉；若大于，则会以空白填充）
sh	可选。被剪切图像的高度（就是裁剪之前的图片高度）
dx	在画布上放置图像的 x 坐标位置
dy	在画布上放置图像的 y 坐标位置
dw	可选。要使用的图像的宽度（就是裁剪之后的图片高度，放大或者缩放）
dh	可选。要使用的图像的高度（就是裁剪之后的图片高度，放大或者缩放）

//访问像素值
imageData = context.getImageData(0, 0, width, height);
//黑白二值化
const data = imageData.data;
// 遍历每个像素点并进行二值化处理
for (let i = 0; i < data.length; i += 4) {
    const r = data[i], g = data[i + 1], b = data[i + 2];
    // 根据RGB值判断该像素点是否为白色（亮度大于等于128） 中
    if ((r > 127 && g > 127 && b > 127)) {
        data[i] = 255; // R通道设置为最大值表示白色
        data[i + 1] = 255; // G通道设置为最大值表示白色
        data[i + 2] = 255; // B通道设置为最大值表示白色
    } else {
        data[i] = 0; // R通道设置为最小值表示黑色
        data[i + 1] = 0; // G通道设置为最小值表示黑色
        data[i + 2] = 0; // B通道设置为最小值表示黑色
    }
}
//修改像素
context.putImageData(imageData, 0, 0);


// 画线：从0,0坐标画一条线到10,10，线宽1，颜色#000
context.beginPath();
context.moveTo(0, 0);
context.lineTo(10, 10);
context.lineWidth = 1;
context.strokeStyle = '#000';
context.stroke();


// 将Canvas转换为Blob再转成file，quality是0-1的压缩精度
this.$refs.canvas.toBlob( (blob) => {
    const file = new File([blob], 'code.jpeg', {
        type: blob.type,
        lastModified: Date.now()
    });
}, 'image/jpeg', quality);

```

+ 二维码识别集成（含扫一扫等）：[html5-qrcode](https://github.com/mebjas/html5-qrcode)
+ 前端图片压缩兼容版:[localResizeIMG](https://github.com/think2011/localResizeIMG)