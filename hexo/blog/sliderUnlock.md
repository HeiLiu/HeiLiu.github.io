---
title: sliderUnlock(iPhone 滑动解锁样式效果)
copyright: true
top: 100
date: 2020-06-05 12:11:09
tags:
---


## 相关属性

### linear-gradient()
> 背景渐变样式、属于一种特殊的 <image> 数据类型，一般适用于 image 可以使用的地方，并不适用于background-color, 而是 background-image

用法：
```css
/* 指定渐变轴的角度 */
linear-gradient(30deg, red, blue);
```
<div style="height: 80px; width: 80px;background-image: linear-gradient(30deg, red, blue);"></div>

```css
/* 直接指定渐变的方向 以及颜色范围 */
background-image: linear-gradient(to bottom, blue 60%, green);
```
<div style="height: 80px; width: 80px;background-image: linear-gradient(to bottom, blue 60%, green);"></div>

linear-gradient 的渐变彩虹效果（更精细的控制）:  
```css
background-image: linear-gradient(to bottom, red 14.28%, orange 28.5%, yellow 42.8%, green 57.1%, blue 71.4%, indigo 85.7%, purple);
```
<div style="height: 80px;background-image: linear-gradient(to bottom, red 14.28%, orange 28.5%, yellow 42.8%, green 57.1%, blue 71.4%, indigo 85.7%, purple);"></div>

```css
background-image: linear-gradient(to right, rgba(0, 0, 0, 0.6) 40%, #fff 50%, rgba(0, 0, 0, 0.6) 60%);
```
<div style="height: 80px; background-image: linear-gradient(to right, rgba(0, 0, 0, 0.6) 40%, #fff 50%, rgba(0, 0, 0, 0.6) 60%);"></div>

### background-blend-mode
> 定义元素的背景图片、以及背景色如何混合

## background-clip
> 设置元素的背景（背景图片或颜色）是否延伸到边框、内边距盒子、内容盒子下面。

| 属性        | 效果                                                                                     |
| ----------- | ---------------------------------------------------------------------------------------- |
| border-box  | 背景色（或图片）延伸到边框（存在边框的时需要将边框设置为虚线（非 soild）或者设置透明度） |
| padding-box | 背景色（或图片）延伸至 padding 区域展示、不到边框                                        |
| content-box | 背景色（或图片）只在 content 区域展示                                                    |
| text        | 背景色（或图片）裁剪成文字前景色、即背景色透上来, 文字需要设置透明度                     |

### background-size
> 设置背景图片的大小 可拉伸可按比例缩放

| 属性         | 效果                                                                                                                         |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| contain      | 缩放背景图片以完全装入背景区，填充所在区域，图片太小会通过repeat的形式填满、不需要repeat可以指定 background-repeat:no-repeat |
| cover        | 缩放背景图片以完全覆盖背景，保持宽高比缩放，可能图片部分看不见区                                                             |
| percentage   | 图片相对背景区的百分比                                                                                                       |
| width height | 指定宽高，按指定大小渲染；只传一个值为宽度，不传height默认 auto                                                              |


### background-image
> 背景图片的展示

```css
background-image: url(png1.png), url(png2.png), rgba(255, 255, 0, 0.5));
```
在绘制时，图像以 z 方向堆叠的方式进行。先指定的图像会在之后指定的图像上面绘制。因此指定的第一个图像“最接近用户”。

然后元素的边框 border 会在它们之上被绘制，而 background-color 会在它们之下绘制。

### background-blend-mode  
> 定义该元素的背景、以及背景元素该如何混合, 混合模式应该按background-image CSS属性同样的顺序定义, 如果混合模式数量与背景图像的数量不相等，它会被截取至相等的数量。    

```css
/* 单值 */
background-blend-mode: normal; // 最终颜色永远是顶层颜色、类似两张不透明的纸重叠在一起
background-blend-mode: hard-light; // 效果类似于在背景层上用前景层打出一片刺眼的聚光灯 滑动解锁文字效果
background-blend-mode: soft-light; // 效果类似于在背景层上用前景层打出一片发散的聚光灯 滑动解锁文字效果

/* 双值，每个背景一个值 */
background-blend-mode: darken, luminosity;

background-blend-mode: initial;
background-blend-mode: inherit;
background-blend-mode: unset;
```
[blend-mode取值类型参考文档]（https://developer.mozilla.org/zh-CN/docs/Web/CSS/blend-mode）


<iframe
     src="https://codesandbox.io/embed/huadongjiesuo-1dpwg?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="滑动解锁"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

## 参考文档 && 链接
[linear-gradient](https://developer.mozilla.org/zh-CN/docs/Web/CSS/linear-gradient)  
[background-blend-mode](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-blend-mode)