---
title: "Re：从零开始の CSS 学习笔记——动画"
subtitle: "CSS 动画"
date: 2020-08-02T01:19:27+08:00
lastmod: 2020-08-02T01:19:27+08:00
draft: false

author: "Sam"
authorLink: "http://liubingxuan.xyz/"

description: "CSS is A*s***e!! 🥴 🤢 🤮"
resources:
- name: "featured-image"
  src: "featured-image.jpg"

tags: ["CSS","笔记"]
categories: ["入门"]
license: "转载请注明出处"
---

浏览器的渲染流程、transform、transition、animation<!--more-->



​	

## 动画的原理

>   人脑的 bug

### 动画

#### 定义

-   由许多静止的画面（帧），
-   以一定的速度（如每秒30张）连续播放时，
-   肉眼因视觉残象产生错觉，
-   而误以为是活动的画面。

#### 概念

-   帧：每个静止的画面都叫做帧
-   播放速度：每秒24帧（影视）或者每秒30帧（游戏）



​	

​	

## 一个最简单的例子

### 将div从左往右移动

>   http://js.jirengu.com/bagow/1/edit?html,css,js,output

+   [JSBin 示例1](http://js.jirengu.com/wotud/3/edit)  通过循环定时器 + 定位 + left 实现动画。控制每隔一小段时间增加 left 值，实现位移
+   [JSBin 示例2](http://js.jirengu.com/rogot/1/edit)  通过延时器 + 添加类名 + transition / transform 实现动画。控制添加类名

#### 原理

-   每过一段时间（用setlnterval做到），
-   将div移动一小段距离，
-   直到移动到目标地点。

#### 注意性能

>   需要先搞懂：[浏览器的渲染步骤](#浏览器渲染过程)，以及 [每个属性会触发什么流程](#每个属性触发什么流程)

+   绿色表示重新绘制（repaint）了

+   CSS渲染过程依次包含布局、绘制、合成
+   其中布局和绘制有可能被省略



​	

### 前端高手不用 left 做动画

#### 用 transform（变形）

+   [JSBin 演示](http://js.jirengu.com/lojiz/1/edit?html,css,js,output)

#### 原理

-   `transform: translateX(0=>300px)`
-   直接修改会被合成，需要等一会修改
-   transition 过渡属性可以自动脑补中间帧

#### 注意性能

>   需要先搞懂：[浏览器的渲染步骤](#浏览器渲染过程)，以及 [每个属性会触发什么流程](#每个属性触发什么流程)

+   transform 优势在于，并没有 relayout（重新布局） 和 repaint（重新绘制）过程
+   transform 比 left 性能好很多（ 因为 left 会依次经过 relayout、 repaint、composite 3个过程）



​	

### 如何查看性能

>   上述 JSBin 示例1/2两种方式，在性能上有什么区别 ？

#### 查看性能的方式

+   开启浏览器的【渲染 Rendering 】>  【画图闪烁 Paint flashing】：突出显示需要重新绘制的页面区域（绿色）
    +   **如果元素发生的重新渲染（绿）的次数多，则更耗性能**

<img src="https://i.loli.net/2020/07/30/86zKIfi5kT3HnNb.png" alt="image-20200730191619715" style="zoom: 67%;" />



+   示例1：使用 setInterval ，控制 left 实现动画

    （位移全程 demo元素 都呈绿色：说明全程都在进行demo元素的重新渲染绘制）

<img src="https://i.loli.net/2020/07/30/Ucpilskq7vNdywJ.gif" alt="left" style="zoom:67%;" />

+   示例2：添加类名，通过 transition + transform 实现动画

    （刷新后，初次渲染呈绿色，移动过程没有发生重新渲染，移动结束的位置重新渲染一次）

    <img src="https://i.loli.net/2020/07/30/chLvHnw4sjBQfJo.gif" alt="transform.gif" style="zoom:67%;" />

+   **总结**：**显然 ，示例1 更耗性能，全程都需要重新渲染**



​	

​	

##  浏览器的渲染原理

>   +   既然讲到这里，提到了性能、渲染，那就深入了解一下
>   +   了解浏览器的渲染流程后，再回头看前面2个示例，可能会更好理解它们的不同

### 参考文章

#### Google 团队写的文章（右上角中文）

-   [渲染树构建、布局及绘制](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction)
-   [渲染性能](https://developers.google.com/web/fundamentals/performance/rendering/)
-   [使用 transform 来实现动画](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count)

#### 查看CSS各属性触发什么

+   [CSSTriggers.com](https://csstriggers.com/)



​	

### 浏览器渲染过程

>   浏览器在获取到 html 和 css 后做了什么？

#### 步骤

1.  根据 HTML 构建 HTML 树（DOM）
2.  根据 CSS 构建 CSS 树（CSSOM）
3.  将两棵树合并成一颗渲染树（render tree）
4.  根据渲染树，进行 Layout 布局（**文档流**、盒模型、计算大小和位置）
    +   先定位：某个标签是否在文档流中…**（文档流的概念非常重要！！）**
    +   定位后，要知道这个标签多高多宽、样式如何…
5.  Paint 绘制（填色：把边框颜色、文字颜色、阴影等画出来）
6.  Composite 合成（根据层叠关系展示画面）



#### 三棵树

>   render tree 就是最终用户看到的树

<img src="https://i.loli.net/2020/07/30/QkBCtAZrgmjcxJa.png" alt="image-20200730202635490" style="zoom: 80%;" />



​	

## 如何更新样式

### 一般我们用 JS 来更新样式

-   比如 `div.style.background='red'`  让内联背景变红色
-   比如 `div.style.display='none'`  让div消失
-   比如 `div.classList.add('red')`  （小白才直接加样式，高手从来只加类名）
-   比如 `div.remove()`直接删掉节点

### 那么这些方法有什么不同吗

-   有三种不同的渲染方式
-   详细看下面  ↓↓



### 三种更新方式

>   使用 JS 来更新样式，要经过哪些步骤 ？
>
>   +   下面有3种代码示例，配合开启浏览器渲染功能，清晰看到执行重新绘制（Paint）的元素
>
>   +   注意 JSBin 中最好**全屏**查看效果，在 iframe 里看可能有问题

<img src="https://i.loli.net/2020/07/30/eLaStTpGBv5wl71.png" alt="image-20200730202524163" style="zoom: 50%;" />

##### 第一种，流程全走一遍

-   [div.remove()](http://js.jirengu.com/jagel/1/edit?html,css,js,output) 会触发当前消失，其他元素 relayout 重新布局

##### 第二种，跳过 layout

+   说明没有改变元素的位置和大小，不需要变动布局

-   比如说：只[改变背景颜色](http://js.jirengu.com/jidam/1/edit?html,css,js,output)，直接 repaint + composite

##### 第三种，跳过 layout 和 paint

+   没有改变位置、大小，也没有改颜色，只需要合成

-   例如：只[改变 transform](http://js.jirengu.com/wusew/1)，则只需 composite 合成
-   注意必须全屏查看效果，在 iframe 里看有问题



​	

### 每个属性触发什么流程

>   CSS变态之处来了：挨个尝试吧

还好，程序员喜欢分享

https://csstriggers.com/  这个网站已经把所有属性全试过了

<img src="https://i.loli.net/2020/07/31/WtaTQbkACBsUJyp.png" alt="image-20200731000339579" style="zoom: 80%;" />

-   Blink：谷歌 Chrome 浏览器的内核（一般只看 Chrome 性能渲染）
-   Gecko：火狐浏览器 Firefox
-   WebKit：苹果 Safari 浏览器
-   EdgeHTML：微软 Edge 浏览器

>   现在可以解释 [为什么](#一个最简单的例子) **前端高手不用 left 做动画**，而用 transform 做动画了
>
>   +   因为执行 left 会触发3个流程：先布局、再绘制、最后合成
>   +   而执行 transform 只会触发 1个流程：只合成

<img src="https://i.loli.net/2020/07/31/aqiuHFsVROeUj9g.png" alt="image-20200731005144963" style="zoom:80%;" />

<img src="https://i.loli.net/2020/07/31/BaYIjDdrhTEyAbo.png" alt="image-20200731004929606" style="zoom:80%;" />



​	

​	

## CSS动画优化

>   可以自己总结一篇博客。面试背不出来，可以让面试官去看博客

>   CSS 性能优化，除了把 left 变成 transform ，还有什么？
>
>   +   面试可能考察，但这个问题**没什么技术含量**，就是背

### 没什么技术含量

答案都在 [Google写的文章](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count) 里，谁看完谁牛 X

<img src="https://i.loli.net/2020/07/31/lNgzP8CATID5Hy6.png" alt="image-20200731002020630" style="zoom:80%;" />

+   [优化 JS 的执行](https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution) （JavaScript）
+   [缩小样式计算的范围并降低其复杂性](https://developers.google.com/web/fundamentals/performance/rendering/reduce-the-scope-and-complexity-of-style-calculations) （优化 Style 过程）
+   [避免大型、复杂的布局和布局抖动](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing)（优化 Layout 布局过程）

+   [简化绘制的复杂度、减小绘制区域](https://developers.google.com/web/fundamentals/performance/rendering/simplify-paint-complexity-and-reduce-paint-areas)（优化 Paint 绘制过程）
+   [坚持仅合成器的属性和管理层计数](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count)（优化 Composite 合成过程）

>   上述每篇文章中的优化方法，都总结在【==TL;DR==（too long don't read 太长不看）】部分
>
>   每条优化点之间，都没什么规律，就依靠死记硬背（面试问到，可能这些点，如果能答出1个，就得满分）

### JS优化

使用 requestAnimationFrame 代替 setTimeout 或 setInterval

### CSS优化

使用 will-change 或 translate（transform）

### 没错

>   没错，完全就是死记硬背！

>   如果面试官问，“ CSS 怎么优化 ”？（通常就是问动画怎么优化，性能上更低耗）
>
>   +   动画尽量使用 will-change 或 translate（transform），不直接使用 left 
>   +   JS 控制的动画中，尽量不使用 setTimeout 和 setInterval，而使用 requestAnimationFrame



​	

​	

##  transform 全解

>   [MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform) 上讲的非常详细

### transform

#### 四个常用功能

-   [位移 translate](#transform之translate)（常用）
-   [缩放 scale](#transform之scale)（常用）
-   [旋转 rotate](#transform之rotate)（做加载动画可能用到）
-   [倾斜 skew](#transform之skew)（不常用）

#### 经验

-   一般都需要配合 transition 过渡 
-   inline 元素不支持 transform，需要先变成 block



​	

​	

### transform之translate

#### 常用写法

>   可以写长度length和百分号percentage， ?表示可省略值

-   `translateX(<length-percentage>)`   （横向）

-   `translateY（<length-percentage>)`    （纵向）

-   `translate(<length-percentage>,<length-percentage>?) `

    -   可省略第二个值，只写第一个值，默认x轴

-   `translateZ(<length>)` （垂直于屏幕的方向）

    -   在三维世界中，才能看出 Z 的变化。

        -   需要配合 perspective 属性，告知浏览器**视点**的位置 来形成三维。
        -   例：**`perspective: 1000px`** 指视点在（位于画面中心）距离屏幕 1000 像素的位置上。

    -   注意：是给父容器设置 perspective

        ```html
        <div class="wrapper">
          <div id="demo"></div>
        </div>
        <style>
          #demo{
            width: 100px;
            height: 200px;
            border: 1px solid red;
            margin: 50px;
          }
          #demo:hover{
            transform: translateZ(50px);  /* 元素在z轴（默认垂直屏幕方向）上的位置 */
          }
          .wrapper{
            perspective: 1000px;  /* 形成三维构图，标注视点位置 */
            border: 1px solid black;
          } 
        </style>
        ```

-   translate3d(x,y,z) 

    +   **`translate3d(50px,50px,200px);`**  同时设定3个轴上的位置

-   [JSBin 演示](http://js.jirengu.com/xidiy/1/edit?html,css,output)

#### 经验

-   要学会看懂 [MDN 的语法格式](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform) 

-   translate(-50%，-50%) 可做绝对定位元素的**居中**

    ```css
    #demo{
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%)
    }
    ```




​	

​	

### transform之scale

#### 常用写法 

-   `scaleX(<number>) `
-   `scaleY(<number>) `
-   `scale(<number>,<number>?)` 
-   [JSBin 演示](http://js.jirengu.com/jucal/1/edit?html,css,output) 

#### 经验 

+   用得较少，因为缩放容易出现模糊
    +   border 也会跟随缩放



​	

​	

### transform之rotate

#### 常用写法

>   rotate 默认以 Z轴为中心轴，进行转动

-   `rotate（[<angle>|<zero>]) `   以Z轴为中心旋转
-   `rotateZ([<angle>|<zero>]) `   以Z轴为中心旋转
-   `rotateX([<angle>|<zero>])  ` 以X轴为中心旋转
-   `rotateY（[<angle>|<zero>]) `   以Y轴为中心旋转
-   [rotate3d](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotate3d) 太复杂，无法用语言表述 
-   [JSBin 演示](http://js.jirengu.com/jiquq/1/edit?html,css,output)

#### 经验 

-   一般用于360度旋转制作 loading 
-   用到时再搜索 [rotate MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotate) 看文档



​	

​	

### transform之skew

#### 常用写法 

-   `skewX([<angle>|<zero>]) `
-   `skewY（[<angle>|<zero>]) `
-   `skew([<angle>I<zero>],[<angle>|<zero>]?) `
-   [JSBin 演示](http://js.jirengu.com/tazer/1/edit?html,css,output)

#### 经验 

-   用得较少
-   用到时再搜 skew MDN 文档



​	

​	

### transform 多重效果

#### 组合使用 

+   **`transform: scale(0.5) translate(-100%, -100%);`**

+   **`transform: none;`** 取消所有



​	

​	

## 实践：用 transform 做红心

### 跳动的心

+   [JSBin](http://js.jirengu.com/nonud/1/edit?html,css,output)

### 心得

-   CSS需要你有想象力，而不是逻辑
-   CSS给出的属性都很简单，但是可以组合得很复杂

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>beating heart</title>
    <style>
      *{
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }
      #heart{
        margin: 100px;
        position: relative;
        /*border: 1px solid black;*/
        display: inline-block;
        transition: all .5s ease;
      }
      #heart:hover{
        transform: scale(1.5);
      }
      #heart>.bottom{
        width: 50px;
        height: 50px;
        background-color: red;
        /*border: 1px solid red;*/
        transform: rotate(45deg);
      }
      #heart>.left{
        width: 50px;
        height: 50px;
        background-color: red;
        /*border: 1px solid red;*/
        border-radius: 50% 0 0 50%;
        transform: rotate(45deg) translateX(31px);
        position: absolute;
        bottom:100%;
        right: 100%;
      }
      #heart > .right {
        width: 50px;
        height: 50px;
        background-color: red;
        /*border: 1px solid red;*/
        border-radius: 50% 50% 0 0;
        transform: rotate(45deg) translate(0,30px);
        position: absolute;
        bottom: 100%;
        left: 100%;
      }
    </style>
  </head>
  <body>
    <div id="heart">
      <div class="left"></div>
      <div class="right"></div>
      <div class="bottom"></div>
    </div>
  </body>
</html>
```



​	

​	

## transition 过渡

>   学习资料：[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition)、[bufan 文档](D:\Bufan\[笔记]\阶段一\03-H5+CSS3\h5+css3.md)

### 作用

+   补充中间帧
    +   已知开头位置，结尾位置，中间运动轨迹自动补齐

### 语法

>   **`transition`** CSS 属性是 [`transition-property`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-property)，[`transition-duration`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-duration)，[`transition-timing-function`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-timing-function) 和 [`transition-delay`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition-delay) 的一个[简写属性](https://developer.mozilla.org/en-US/docs/CSS/Shorthand_properties)。
>
>   >   [JSBin 示例](http://js.jirengu.com/wasiv/1/edit?html,css,js,output)

-   ```css
    transition: 属性名 时长 过渡方式 延迟
    ```

    **`transition: left 200ms linear `**   属性名是指要给哪个属性添加过渡效果

-   可以用 all 代表所有属性：width | height | left | right | margin-right  …… 

    **`transition: all 200ms `**

-   可以用逗号分隔两个不同属性 

    **`transition: left 200ms, top 400ms `**

-   时长 ：支持 秒 s  和  毫秒 ms 单位。0.5s 可以写成  .5s

-   过渡方式有：linear（线性匀速） | ease（默认值：缓动） | ease-in | ease-out | ease- in-out | cubic-bezier | step-start | step-end | steps

    -   [具体含义](https://developer.mozilla.org/zh-CN/docs/Web/CSS/timing-function)要靠数学知识
    -   https://cubic-bezier.com/   测试运动曲线



### 注意

>   不学常态，学变态

#### 并不是所有属性都能过渡

-   一个元素，切换可见状态

    -   使用 display: none <=> display: block 没法过渡，会闪现、闪隐【元素消失，不占位置】

    -   使用 opacity: 0 <=>  opacity: 1   透明度控制可见状态【可实现过渡效果，**缺点**是元素消失仍占位置】

    -   **推荐**使用 **`visibility: hidden`** <=> **`visibility: visible`** （不要问为什么）【缺点没法过渡、元素消失仍占位置】

        ```js
        button.onclick = () => {
          demo.classList.add('end');
          /* 解决隐藏后仍占位问题：延迟1s后，将元素删除 */
          /* 方法一：使用延时器 */
          setTimeout(() => { demo.remove(); } ,1000)  
          /* 方法二: on事件可能有bug，推荐使用事件监听器 */
          demo.ontransitionend = () => { demo.remove(); }  /* on事件 */
          demo.addEventLisener('transitionend', () => { demo.remove(); })   /* 事件监听器 */
        }
        ```

-   display 和 visibility 的区别

    -   https://www.cnblogs.com/zrenj/p/9785835.html

        <img src="https://i.loli.net/2020/07/31/9sFSKeyDGiZ2Va1.png" alt="image-20200731220756239" style="zoom: 5%;" />

-   background 颜色可以过渡吗？可以 。    

    -   [查看示例](http://js.jirengu.com/wasiv/3/edit?html,css,js,output)

-   opacity 透明度可以过渡吗？ 可以 。  

    -   [查看示例](http://js.jirengu.com/wasiv/5/edit?html,css,js,output)
    -   **不推荐用透明度控制显示隐藏，推荐 visibility**
    -   opacity: 0 <=>  opacity: 1   可实现过渡效果，**缺点**是元素消失仍占位置，可通过 js 控制 remove() 该元素



​	

### 过渡必须要有起始

>   起始：指一个属性的开始是一个值，该属性的结尾也有一个值。这样才能实现某属性的属性值的变化过渡，中间过渡的效果浏览器会自动补充

-   一般只有一次动画，或者两次
    -   一次：指只有进入动画
    -   两次：①进入动画、②离开动画
-   比如 hover 和 非 hover 状态的过渡，就是两次动画



​	

​	

## 如果除了起始，还有中间点，怎么办

>   例如：从红色，先变黄色，最后再变绿色，怎么实现

有如下两种办法

### 方法① 使用两次 transform

-   流程：   .a =\== transform =\==> .b    然后   .b =\== transform ===> .c

-   如何知道到了中间点呢？

    -   用 setTimeout 或者监听 transitionend 事件。

        >   给元素添加新的类名：执行第二段 transform 效果。
        >
        >   -   注意：第二段 transform 中必须包含第一段动画效果，不然执行第二段动画可能还原初始位置，有 bug，可自行测试。

    -   [JSBin 示例](http://js.jirengu.com/vehuz/2/edit)

### 方法② 使用 animation

-   声明关键帧
-   添加动画
-   [JSBin 示例](http://js.jirengu.com/peran/1/edit?html,css,output)



​	

​	

## animation 动画

###  提问

>   如何让动画停在最后一帧？

-   搜索 css animation stop at end

    -   网友给出的答案是：加个 forwards

        ```css
        animation: xxx 1.5s forwards;
        ```

-   [JSBin 演示](http://js.jirengu.com/lodoy/1/edit?html,css,output)



​	

​	

### @keyframes 完整语法

#### 标准写法

-   搜索 [keyframes MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes) 讲得很清楚
    -   一种写法是 from to ：只支持两种状态
    -   另一种写法是百分数：支持添加 n 个帧状态

<img src="https://i.loli.net/2020/07/31/LINXuQmsd6Pzaj7.png" alt="image-20200731223012625" style="zoom: 60%;" />

<img src="https://i.loli.net/2020/07/31/AYK8w2osjMNpSFh.png" alt="image-20200731223033136" style="zoom: 67%;" />



​	

### animation 缩写语法

>   [animation MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation)

>   **animation** 属性是 [`animation-name`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-name)，[`animation-duration`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-duration), [`animation-timing-function`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-timing-function)，[`animation-delay`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-delay)，[`animation-iteration-count`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-iteration-count)，[`animation-direction`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-direction)，[`animation-fill-mode`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-fill-mode) 和 [`animation-play-state`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation-play-state) 属性的一个简写属性形式。
>
>   [Jirengu 视频讲解](https://xiedaimala.com/tasks/597dbc7b-15e1-4f27-b814-d2151a63c34b/video_tutorials/046dd8ef-8f6f-4051-a233-d4c997113e24)
>
>   [JSBin 演示](http://js.jirengu.com/gaxed/2/edit?html,css,js,output)

```css
animation: 时长 | 过渡方式 | 延迟 | 次数 | 方向 | 填充模式 | 是否暂停 | 动画名 ;  /* 位置任意 */
```

-   时长：1s 或者 1000ms 

-   过渡方式：跟 transition 取值一样，如 linear 。默认是 ease 先快后慢

-   延迟时间：1s 或 1000ms

-   次数：3 或者 2.4 或者 [infinite]()（无限次） 

-   方向：reverse | **[alternate]()**（交替，非常适合做加载动画） | alternate-reverse

-   填充模式：none | [forwards]()（保持在动画终点位置） | backwards | both

-   是否暂停：paused | running  

    ```js
    pauseBtn.onclick = () => { demo.style.animationPlayState = 'paused' } 
    /* 点击按钮，暂停demo元素的动画 */
    ```

-   更多属性值的效果，需要自己尝试。[JSBin 演示](http://js.jirengu.com/gaxed/3/edit)

-   以上所有属性都有对应的单独属性

    -     **animation** 只是这些单独属性的**缩写**，上述效果可以通过单独的属性设置

 

​	

​	

## 实践：用 animation 做红心

>   [JSBin 示例](http://js.jirengu.com/hosug/1/edit?html,css,output)

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>beating heart-animation</title>
    <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }
      @keyframes beating {
        0% {transform: scale(1)}
        /*25% {transform: scale(1.25)}*/
        /*50% {transform: scale(1.5)}*/
        /*75% {transform: scale(1.25)}*/
        100% {transform: scale(1.5)}
      }
      #heart {
        /*border: 1px solid black;*/
        display: inline-block;
        position: relative;
        margin: 100px;
      }
      #heart:hover {
        animation: beating .5s ease infinite alternate;
      }
      #heart > .left {
        position: absolute;
        bottom: 50px;
        left: -50px;
        width: 50px;
        height: 50px;
        /*border: 1px solid red;*/
        background: red;
        border-radius: 50%;
        transform: rotate(45deg) translateX(43px);
      }
      #heart > .right {
        position: absolute;
        bottom: 50px;
        right: -50px;
        width: 50px;
        height: 50px;
        /*border: 1px solid red;*/
        background: red;
        border-radius: 50%;
        transform: rotate(45deg) translateY(45px);
      }
      #heart > .bottom {
        width: 50px;
        height: 50px;
        /*border: 1px solid red;*/
        background: red;
        transform: rotate(45deg);
      }
    </style>
  </head>
  <body>
    <div id="heart">
      <div class="left"></div>
      <div class="right"></div>
      <div class="bottom"></div>
    </div>
  </body>
</html>
```

