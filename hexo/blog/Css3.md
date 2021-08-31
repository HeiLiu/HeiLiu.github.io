---
title: Css3属性 
date: 2018-05-04
tags: Css3
categories: 前端
---

## text-shadow属性  
**介绍:**

`text-shadow`属性可以给页面上的文字增添阴影效果，`text-shadow`在Css2.1的时候是被删除了的一个属性，但是呢在 3.0 的Css中又恢复了使用 
    
### 使用方法   
 
```css
  text-shadow : length length length color 
```
  <br/>注：前两个参数在使用的时候必须给定
- 第一个`length`表示阴影离开文字横方向的距离  
- 第二个`length`表示阴影离开文字纵方向的距离  
- 第二个`length`表示阴影模糊半径 即模糊范围  
- `color` 表示阴影颜色 可以放在三个`length`之前 也可以放在之后 如果不给值 则使用元素默认`color`   

  $ 指定多个阴影： 每个阴影用逗号隔开 
```css
  text-shadow : 15px 15px 5px #000,30px,30px,5px #f60; 
```
## background-size属性
### 介绍: 
  在Css3中可以使用`background-size`来指定背景图像的尺寸  
### 使用方法 
 ```css 
  background-size: auto || length || percentage || cover || contain
```
- `auto`: 默认值 保持背景图片的原始宽高比  
- `length`: 设置背景图片的宽度和高度，如果只设置一个值，则第二个值会被设置为`auto` 按宽高比进行放大缩小   
- `percentage`: 以父元素的百分比来设置宽度和高度，如果只设置一个值 同上  
- `cover`: 此值是将图片放大，以适合铺满整个容器，这个主要用在当图片小于容器又无法使用background-repeat来实现，就采用`cover`将背景图片放大到适合容器的大小，但是会使图片失真  
- `contain`: 与`cover`相反，将背景图片缩小以适合铺满整个容器，这个主要用在当背景图片大于元素容器时，而又需要将背景全部显示出来，就可以用`contain`将图片缩小到适合容器的大小，这种方法同样会使图片失真 

## 在一个元素中显示多个背景图片  
#### 介绍:
   在Css3一个元素可以显示多个背景图像，还可以将多个背景图像进行重叠显示，这样对背景中所用素材调整变得更加容易。  
#### 使用方法 
```css
background-image:url(1.png),url(2,png),url(3.png);
```
图层的排序方法: 浏览器中显示时叠放的顺序是从上往下指定的，第一个图片放在最上面，最后指定的放在下面  

## Css3的变形功能
### transform属性  
 **介绍:**  
    在Css3中可以利用`transform`属性来实现文字或图像的旋转、缩放、倾斜、和移动。但是需要做兼容性处理，如下  
    ```css
    -webkit-transform: rotateX(60deg);
    -moz-transform: rotateX(60deg);
    -ms-transform: rotateX(60deg); /*IE9*/
    -o-transform: rotateX(60deg);  
    ```
 - rotate(旋转)：
    - `rotate(60deg)`;顺时针旋转 `deg`是Css3中的角度单位
    ```css
    -webkit-transform: rotateX(60deg);
    ```
    - rotateX(angle) rotateY(angle) rotateZ(angle)
        绕对应的轴进行3D旋转
    - `rotate3D(X,Y,Z,deg)`	定义 3D 旋转
 - scale(缩放)：  
 `transform:scale(值)` 值所指的是缩(小)放(大)倍率 如果值为负数并没有效果 纯属无聊
    - `scale(x,y)` 使元素在x轴 y轴方向同时缩放
    - `scaleX(.5)` 你应该懂得
    - `scaleY(.5)` 同上
 - skew(倾斜)  
  `transform:skew(deg)` 倾斜角度
     - `skew(x,y)` 元素在水平和垂直方向上同时倾斜 只有一个参数时，只在水平方向上倾斜  
     - `skewX(x)` 元素仅在水平方向倾斜  
     - `skewY(y)` 元素在垂直方向倾斜  
 - `translate`(值) 指定移动的距离  负值即反方向移动
    - `translate(x,y)`;在x轴和y轴同时移动，只有一个参数时，仅在水平方向移动;  
    - `translateX(x)`;x轴方向移动  
    -` translateY(y)`;y轴方向移动
 - transform-origin 改变元素基点  
    属性使用：transform-origin:bottom;
    各个基点参考如图：
    ![transform-origin](https://github.com/HeiLiu/markdown_source/blob/master/images/transform-origin.jpg)
 - 对一个元素使用多种变形的方法：  
    transform：方法一，方法二，方法三..;
    ```css 
     /*同样也需要做兼容性处理*/
    transform: translate(50px),rotate(60deg),scale(2);
    ```
## Css3的动画功能  
[demo](https://github.com/HeiLiu/markdown_source/blob/master/demo/fzdh.html)
### `transition`  支持从一个属性平滑过渡到另外一个属性  
- 语法:
```css
  transition: property duration timing-function delay;
```
- `transition` 主要包含四个属性值:  
- 执行变换的属性: `transition-property`,属性规定应用过渡效果的CSS属性的名称。(当指定的CSS属性改变时，过渡效果将开始)值有三个类型:
            
A、`none` 没有属性会获得过渡效果。  
B、`all`所有属性都将获得过渡效果。  
C、`property`定义应用过渡效果的CSS 属性名称列表，列表以逗号分隔。
              
- 变换延续的时间: `transition-duration`规定完成过渡效果需妻花费的时间(以秒或毫秒计》，默认值0没有效果
- 在延续时间段，变换的速率变化transition-timing-function
值:  
```  
A. ease:  (逐渐变慢) 默认值，ease函数等同于贝塞尔曲线(0.25,0.1,0.25,1.0).  
B. linear:  (匀速)，linear 函数等同于贝塞尔曲线(0.0,0.0,1.0,1.0).  
C. ease-in: (加速)，ease-in 函数等同于贝塞尔曲线(0.42,0,1.0,1.0).  
D. ease-out:  (减速)，ease-out 函数等同于贝塞尔曲线(0,0,0.58,1.0).  
E. ease-in-out :  (加速然后减速)，ease-in-out 函数等同于贝塞尔曲线(0.42,0,0.58,1.0)  
F. cubic-bezier(n,n,n,n)在cubic-bezier 函数中定义自己的值。可能的值是0 至1之间的数值。
```

### animation  支持通过关键帧的指定来在页面上产生更复杂的动画效果  
 **用 transition和Animations的区别 :**

`transition`和`Animations`的区别在于，`transition`只能通过指定属性的开始值与结束值，然后通过两属性值之间进行垂滑过渡的方式来实现动画效果，所以transition不能实现复杂的动画效果，而Animations功能是是通过关键幀以及每个关键帧中的属性值来实现更为复杂的动画效果。  
    
   - Animations的使用方法:  
    [参考我的飞机Demo更好哦](https://github.com/HeiLiu/markdown_source/tree/master/demo/fir.im)  
    ```css
    @-webkit-keyframes 关键帧合集名称{  
        创建关键帧的代码  
    }  
    0%~100%{  
        本关键帧中的样式   
    }   
    // 关键帧创建好了之后，还要在元素的样式中使用该关键帧。方法如下: 
    元素{
      - webkit-animation-name :关键帧合集名称 ;
      - webkit-animation-duration:5s ;
      - webkit-animation-timing-function :linear;-webkit-animation-iteration-count:infnite  
    }
    ```css
      -webkit-animation-name 指定合集名称，  
      -webkit-animation-duration 整个动画执行完成需要的时间，  
      -webkit-animation-timing-function 实现动画的方法
      
      -webkit-animation-iteration-count 属性的属性值设定为某个整数值，那么这个动画播放的次数就等于这个整数值(infinite是无限循环播放)。
    ```

实现动画的方式:    
  A、linear: 匀速进行.  
  B、ease-in: 开始速度很慢，然后沿曲线进行加快，  
  C、ease-out: 开始速度很快，然后沿着曲线进行减速.  
  D、ease: 开始时速度很快，然后沿着曲线进行减速，然后再沿着曲线加速，  
  E、ease-in-out: 开始时速度很慢，然后沿着曲线进行加速，然后再沿着曲线减速.

也可以以合集的形式进行样式书写：  
```css
animation: name duration timing-function delay iteration-count direction;
```







