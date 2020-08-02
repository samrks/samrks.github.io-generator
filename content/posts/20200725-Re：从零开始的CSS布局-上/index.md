---
title: "Re：从零开始の CSS 学习笔记——布局（上）"
subtitle: "CSS 布局篇（上）"
date: 2020-07-25T16:16:55+08:00
lastmod: 2020-07-25T16:16:55+08:00
draft: false

author: "Sam"
authorLink: "http://liubingxuan.xyz/"

description: "CSS is A*s***e!! 🥴 🤢 🤮"
resources:
- name: "featured-image"
  src: "featured-image.jpg"
- name: "featured-image-preview"
  src: "featured-image-preview.jpg"

tags: ["CSS","笔记"]
categories: ["入门"]
license: "转载请注明出处"
---

CSS 布局篇（上）： Float 布局、Flex 布局 。  <!--more-->

​		

​		

Float 示例代码  http://js.jirengu.com/wiwuh/3/edit?html,css,output

Flex 示例代码  http://js.jirengu.com/pakez/2/edit?html,css,output

Flex 青蛙游戏  https://flexboxfroggy.com/#zh-cn

​		

## 布局是什么

>   把页面分成一块一块，按左中右、上中下等排列



### 布局分类

#### 两种

-   固定宽度布局，一般宽度为960/1000/1024px （淘宝pc）
-   不固定宽度布局，主要靠文档流的原理来布局（常用在移动端、响应式，会跟随设备宽度变化）

#### 还记得吗

-   文档流本来就是自适应的，不需要加额外的样式

#### 第三种布局

-   响应式布局
    -   意思就是PC上固定宽度，手机上不固定宽度
    -   也就是一种混合布局



​		

### 布局的两种思路

#### 从大到小

-   先定下大局
-   然后完善每个部分的小布局

#### 从小到大

-   先完成小布局
-   然后组合成大布局

#### 两种均可

-   新人推荐用第二种，因为小的简单
-   老手一般用第一种，因为熟练有大局观



​		

### 布局需要用到哪些属性

>   不多哗哗，直接给你所有套路

以前经常说 DIV+CSS 布局，但是现在已经无意于用 DIV 了，就说用 CSS 布局

+   main、header、footer、nav、aside … 这些标签的出现，已经可以代替 div 了

需要兼容 IE9 吗

  +   不用，只做手机页面（闲鱼），阿里巴巴在顺序手机时代
  +   很老的手机产品要兼容吗、兼容最新浏览器吗？

<img src="https://i.loli.net/2020/07/22/9luaMYvVUFhitP5.png" alt="image-20200722180851787" style="zoom: 67%;" />



​			

​			

## Float 布局

>   float 主要是针对 IE 的，而现在公司基本不需要兼容 IE6789。

### 步骤

1.   子元素上加 ` float: left`  和 ` width`

2.   在父元素上加  `.clearfix`（清除浮动的影响）

     ```html
     <!-- header中没有文档流元素，子元素都浮动了(脱离文档流) ，所以header的高度为0 -->
     <!-- 添加 clearfix 后，可以清除浮动的影响 -->
     <header class="clearfix">    
       <div class="logo">XDML</div>
      	<nav>导航</nav>
     </header>
     
     <style>
     	.logo{float: left;...}   /* 脱离文档流 */
     	nav{float: left;...}
     </style>
     ```

     ```css
     .clearfix:after{    
       content: '';
       display: block;
       clear: both;
     }
     /* 背过 clearfix 的写法 */
     ```

     

### 经验

-   有经验者会留一些空间或者最后一个不设 width （或者可以给个 `max-width: xxxpx;`）

-   不需要做响应式，因为手机上没有IE，而**这个布局是专门为 IE 准备的**

-   IE6/7 存在双倍 margin bug（给浮动元素设置 margin: 10px 在 IE6/7 中实际距离会变成 margin: 20px 的效果）

    -   解决办法有两个  

        -   一是将错就错，针对 IE6/7 把 margin 减半

            ```css
            .logo{
              float: left;
              margin-top: 10px;  /* 其他浏览器 只能识别这句，无法识别下面属性 */
              _margin-top: 5px;  /* IE6/7在属性前加 下划线 或 星号 都能识别 */
            }
            ```

        -   二是神来一笔，再加一个 `display: inline-block`

            ```css
            .logo{
              float: left;
              margin-top: 10px; 
              display: inline-block;  /* 微软说：IE6/7遇到margin乘2的bug，就添加这句 */
            }
            ```

            

    -   为什么可以这样？你问我，我问谁…



​				

### 实践

#### 不同布局

-   用 float 做两栏布局（如顶部条）
-   用 float 做三栏布局（如内容区）
-   用 float 做四栏布局（如导航）
-   [用 float 做平均布局](#float实现平均布局)（如产品列表展示区）—— **负margin**
-   曾经淘宝的前端发明了双飞翼布局，不要学，已过时代码

#### 经验

-   加上头尾，即可满足所有PC页面需求
-   手机页面傻子才用float
-   float要程序员自己计算宽度，不灵活
-   float用来应付IE足以



#### 技术总结

>   [JSbin 演示](https://jsbin.com/xihinip/edit?html,css,output)，总结如下

##### outline

>   现象描述：计算宽度时，内部3个元素的宽度和=300，外层容器=300，但是还会把最后一个元素挤下去，说明子元素的宽度超出容器的宽度。

+   原因可能是，虽然设定容器宽度为 300，但是容器添加了**边框** border : 1像素，所以容器的内容区域的宽度实际只有 298 px。(仅限 border-box 情况)
+   解决：
    +   把容器的边框删了
    +   改用 ` outline: 1px solid red;`  outline 是在外侧的，不占内部区域，但是outline 样式上会有点奇怪
    +   改用 背景颜色 标识区域

##### 居中

```css
/* margin: 0 auto;  下面两句的写法更好，这句还覆盖了margin上下的距离，css原则是不要写多余的 */    
margin-left: auto;
margin-right: auto;
```

>   只有==块级元素==，才能使用 margin … auto，实现居中。





##### float实现平均布局

>   添加一个 父元素 x ，进行 ==负margin== 操作（这个词基本都是高手才懂，面试说道这几个字，平均布局这块就一定能过）
>
>   注意：添加 父元素后，clearfix 的位置也需要相应移动到，浮动元素的直属父级上

```html
<div class="imageList">
  <div class="x clearfix">
    <div class="image"></div>
    <div class="image"></div>
    <div class="image"></div>
    <div class="image"></div>
    <div class="image"></div>
    <div class="image"></div>
    <!--<div class="image"></div>-->
    <!--<div class="image"></div>-->
  </div>
</div>
<style>
  .imageList {
    outline: 1px solid green;
    width: 800px;
    margin-left: auto;
    margin-right: auto;
    margin-top: 10px;
  }
  .imageList > .x > .image {
    width: 191px;
    height: 191px;
    background-color: #555;
    border: 1px solid red;
    float: left;
    margin-bottom: 10px;
    margin-right: 12px;
  }
  .imageList > .x {
    margin-right: -12px;
  }
</style>
```

<img src="https://i.loli.net/2020/07/22/M1EroiTwDPnmkdv.png" alt="image-20200722210338975" style="zoom:80%;" />



​	

​	

## 缩写

>   html、css  （√）
>
>   navigator  --->   nav   （√）
>
>   +   不能缩写未经约定、达成统一的单词
>       +   content   —>  cnt    （x）     container 也可以缩写成 cnt  容易误会，所以不要用



​	

​	



## Flex 布局

-   教程（来自 [CSS Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/))
-   把教程过一遍，然后忘掉
-   完成 [Flex青蛙游戏](https://flexboxfroggy.com/)
-   开始用flex！



### 容器 container 有哪些属性

+   container ：表示容器，一般用于做父元素
+   items ：表示容器里面的、直接的子元素，就称为 items（项）

<img src="https://i.loli.net/2020/07/22/3KpAR2JtcGZ61w8.png" alt="image-20200722214411075" style="zoom:50%;" />



>   以下都是 container 的样式 

​	

#### 让一个元素变成 flex 容器

只有下面两种写法：

```css
.container{
  display: flex;     /* 或 display: inline-flex; */
}
```

​	

#### 改变 items 流动方向（主轴）

>   默认，所有项都会挤在主轴，主轴占满会平均压缩每项宽度，以保证在主轴存放下所有项

<img src="https://i.loli.net/2020/07/22/ibdjLPJTMXuHgF1.png" alt="image-20200722220930275" style="zoom: 67%;" />

```css
.container{
  display: flex;
  border: 1px solid red;
  flex-direction: row;  /* 【默认值】横向（从左到右） */
  flex-direction: column;  /* 纵向（从上到下） */
  flex-direction: row-reverse; /* 横向反向（从右到左） */
  flex-direction: column-reverse; /* 纵向反向（从上到下） */
}
```

​	

#### 改变折行

<img src="https://i.loli.net/2020/07/22/3LuoRTPUEdCzAbN.png" alt="image-20200722221103546" style="zoom: 67%;" />

```css
.container{
  display: flex;
  border: 1px solid red;
  flex-direction: row;  /* 默认横向 */
  flex-wrap: wrap-reverse;  /* 反向折行, 效果如下图，基本没用 */
}
```

![image-20200722221520477](https://i.loli.net/2020/07/22/w4bGQOn61JiczFu.png)

>   `flex-direction`和`flex-wrap`两个属性经常会一起使用，所以有缩写属性`flex-flow`。这个缩写属性接受两个属性的值，两个值中间以空格隔开。
>
>   举个例子，你可以用`flex-flow: row wrap`去设置行并自动换行。

​	

​	

#### 主轴对齐方式

>   默认主轴是 横轴
>
>   除非你改变了 flex-direction 方向

<img src="https://i.loli.net/2020/07/22/YeMQbNduPcE3kOr.png" alt="image-20200722222730578" style="zoom:53%;" /><img src="https://i.loli.net/2020/07/22/pTDLlXEoeV5SWPr.png" alt="image-20200722222746237" style="zoom:50%;" />

```html
.container {
  justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
}
```

+   space-around： 每项左右两边的空间一样
+   space-evenly：每项间距一样
+   space-between：把空间放到中间，元素分布两边



​	

#### 次轴对齐

>   默认次轴是 纵轴

```css
.container{
  align-items: stretch [默认值] | flex-start | flex-end | center | baseline（不需要） 
}
```

<img src="https://i.loli.net/2020/07/22/jtJ5sOHKZuv7bVf.png" alt="image-20200722224320410" style="zoom: 50%;" />

+   stretch【默认值】 

    +   默认所有 items 的高度 与 高度最高的 item 保持一致
    +   如下图，3个 item 都与 2 号 item 一样高

     <img src="https://i.loli.net/2020/07/22/APYu5qhUCQaHKT3.png" alt="image-20200722223835192" style="zoom: 67%;" />

+   flex-start

    <img src="https://i.loli.net/2020/07/22/5D3ACg9KILYpZWV.png" alt="image-20200722223941301" style="zoom:67%;" />

+   flex-end

    <img src="https://i.loli.net/2020/07/22/ZdtvSzK4gYhVsEO.png" alt="image-20200722224034230" style="zoom:67%;" />

+   center

    <img src="https://i.loli.net/2020/07/22/Zy7TOKQ4rxJuV6B.png" alt="image-20200722224101576" style="zoom:67%;" />

​    

#### 多行分布

>   很少用到

>   +    默认平均分  ：  `align-content: stretch` (如图)
>
>        <img src="https://i.loli.net/2020/07/22/NKhIu6SznsiMoC2.png" alt="image-20200722225018920" style="zoom:80%;" />

```css
.container{
  display: flex;
  border: 1px solid red;
  flex-wrap: wrap;
  height：400px；
  align-content: flex-start;   /* 全部集中到顶部 */
  align-content: flex-end;   /* 全部集中到底部 */
  align-content: center;   /* 全部集中到中间 */
  align-content: space-between;
  align-content: space-around;
}
```

<img src="https://i.loli.net/2020/08/02/gtqQKAMBvWUfDwY.png" alt="image-20200722224538109" style="zoom:50%;" />



​	

​	

### flex item 有哪些属性

>   以下都是 item 的属性

#### item 上加 order

+   **默认 order 为 0**

+   指定 order 后，item 会按照 order 顺序从小到大排列（可以指定为负数）

    <img src="https://i.loli.net/2020/07/23/92hZAYkyrN4Pd6j.png" alt="image-20200723102111806" style="zoom: 33%;" />

    ```html
    <div class="container">
      <div class="item">1</div>
      <div class="item">2</div>  
      <div class="item">3</div>
      <div class="item">4</div>
    </div>
    
    <style>
      .container{
        display: flex;
        border: 1px solid red;
      }
      .item{
        width:50px;
        height:50px;
        border: 1px solid green;
      }
      .item:first-child{ order: 100; }    /* 最后 */
      .item:nth-child(2){ order: 2; }			
      .item:nth-child(3){ order: 3; }
      .item:last-child{ order: 1; }    /* 最前 */
    </style>
    ```

    <img src="https://i.loli.net/2020/07/23/cSiPkYbCKuVWaMx.png" alt="image-20200723125610356" style="zoom:67%;" />



​	



#### item 上加 flex-grow

>   用于分配多余的空间（控制变胖）
>
>   +   flex-grow： 默认为 0.   就是 item 宽度由内容撑开，没内容的话宽度就是0，不会占用多余的空间
>   +   给 item 设置 flex-grow  值为 n （>0），就是将分配多余空间给当前 item 占 n 份。
>       +   如果一共有3个item，那就平均分配多余空间，每个 item 占 n/3.

<img src="https://i.loli.net/2020/07/23/RlIrbfMAQzu7Ej5.png" alt="image-20200723125746083" style="zoom: 33%;" />

+   当我们不给 item 设置宽度时，item 的宽度是能有多窄有多窄（宽度由内容撑开）

    <img src="https://i.loli.net/2020/07/23/v1BTm7x6Wyz3kGp.png" alt="image-20200723130130864" style="zoom: 67%;" />

+   实现宽度能有多宽有多宽，就给 item 添加 flex-grow

    ```css
    .item{
      flex-grow: 1;    /* 每一个 item 平均分配宽度，来占满多余的空间（不是占满整行空间） */
    }
    ```

    <img src="https://i.loli.net/2020/07/23/oOfiFTV7zcPualp.png" alt="image-20200723130354832" style="zoom: 67%;" />



+   需求：从多余的空间中，给2,3 的宽度占 2 份;   给 1,4 占 1 份 空间

    ```css
    .container{
      display: flex;
      border: 1px solid red;
    }
    .item{
      height:50px;
      border: 1px solid green;
    }
    .item:first-child{ flex-grow: 1; }
    .item:nth-child(2){ flex-grow: 2; }
    .item:nth-child(3){ flex-grow: 2; }
    .item:last-child{ flex-grow: 1; }
    ```

    <img src="https://i.loli.net/2020/07/23/pVKP5kSAimgU1eF.png" alt="image-20200723130902817" style="zoom: 67%;" />




##### 经验

+   当 3 栏布局，如下
    +   只给【导航】设置**` flex-grow: 1`**（实现导航宽度的响应式），logo、头像固定宽度

```html
<div class="container">
  <div class="item">logo</div>
  <div class="item">导航</div>  
  <div class="item">头像</div>
</div>

<style>
  .container{
    display: flex;
    border: 1px solid red;
  }
  .item{
    height:50px;
    border: 1px solid green;
  }
  .item:nth-child(2){   
    flex-grow: 1; 
  }
</style>
```

<img src="https://i.loli.net/2020/07/23/XSOihn8bzDWPjfI.gif" alt="flex-grow" style="zoom:50%;" />

​	



#### flex-shrink 控制如何变瘦

>   当界面不断变窄，无法存放每项的给定宽度时，每项都需要变窄，flex-shrink 就控制谁瘦的多，谁瘦的少
>
>   +   默认是1（所有item平均收缩，要缩一起缩）
>   +   一般写 **`flex-shrink: 0;`**  防止变瘦（被设置为 0 的这一项，就算空间不够时，也不会收缩。要缩别找我）

```css
.item{
  width: 150px;  /* 合计宽度最小450px */
  height:50px;
  border: 1px solid green;
  flex-grow: 1;  /* 每项会均分多余空间 */
}
.item:first-child{
  flex-shrink: 1;
}
.item:nth-child(2){ 
  flex-shrink: 50;   /* 2 的收缩比例较大 */
}
.item:last-child{
  flex-shrink: 1;
}
```

+   当宽度缩小达到450px以内，每项宽度不足，此时每项会开始收缩， flex-shrink 值越大，则收缩的越大，flex-shrink 值越小，越不会受到收缩的影响
    +   如下图，宽度收缩450px以内，【导航】最先开始发生了较大的收缩，因为设定了较大的 flex-shrink 值
    +   【logo】和【头像】版块，基本不收缩

<img src="https://i.loli.net/2020/07/23/i2kQOVLXDHPygrh.gif" alt="flex-shrink" style="zoom: 67%;" />



​	

#### flex-basis 控制基准宽度

>   用法：
>
>   +   默认是 auto（与 item 的 width 值保持一致）
>   +   指定宽度：`flex-basis: 100px;`  相当于指定了 width 值

这个属性方方也比较迷：不是很重要的属性

+   可以直接用 width 来代替这个属性



​	

#### 缩写成 flex

>    flex  相当于  flex-grow  flex-shrink   flex-basis
>
>    方方一般不写缩写，容易记错位置

+   flex  只有以下 4 种形式的写法

<img src="https://i.loli.net/2020/08/02/d3olkwf4AvPcQUp.png" alt="image-20200723135453043" style="zoom:67%;" />

```css
.item:first-child{
  flex: 1 1 100px;    /* grow-1，shrink-1，宽100px */
}
.item:nth-child(2){
  flex: 1 100 100px;    /* grow-1，shrink-100，宽100px */
}
.item:last-child{
  flex: 1 1 100px;    /* grow-1，shrink-1，宽100px */
}
```

+   上述，3个item，宽度为100px
    +   grow撑开时每个item平均占满所有多余空间
    +   宽度不足时，2 号主要进行收缩



​	

#### align-self 定制 align-items

>   用的很少
>
>   +   默认在垂直方向上，都是顶端对齐的
>   +   align-self  可以让某一个 item，在垂直方向上，特例独行的展示（指定一个特别的对齐方式）

<img src="https://i.loli.net/2020/07/23/RdDKx3glnpVvi5B.png" alt="image-20200723140835156" style="zoom: 33%;" />

实现：单独设置，最后一个 item 底部对齐

![image-20200723140535900](https://i.loli.net/2020/07/23/QZ5KnmLkF3zpUMB.png)

​	

​	

### 重点

-   记住这些代码 
-   **`display: flex`**     开启flex布局
-   **`flex-direction: row / column `**   主轴是横向还是纵向
-   **`flex-wrap: wrap `**   空间不足时是否换行
-   **`just-content: center / space-between`**   主轴方向上的对齐方式：居中/分开
-   **`align-items: center  `**    次轴方向上的对齐方式：居中，顶，底
-   工作中基本只用这些



​	

​	

### 实践

>   [JSBin 演示](https://jsbin.com/yoguyuh/edit?html,css,output)

#### 不同布局

-   用 flex 做两栏布局
-   用 flex 做三栏布局
-   用 flex 做四栏布局
-   用 flex 做平均布局 —— [负 margin](#负margin方案)
-   用 flex 组合使用，做更复杂的布局
-   [JSBin 代码](https://jsbin.com/hehekod/edit?html,css,output)

#### 经验

-   永远不要把 width 和 height 写死，除非特殊说明
    -   PC端通常可以写死。移动端不能写死，需要适配各种尺寸：平板/手机…
-   用  min-width  /  max-width  /  min-height  /  max-height
-   flex 可以基本满足所有需求
-   flex  和  `margin-xxx : auto`  配合有意外的效果——例：[左右布局](#左右布局)



​	

#### 什么叫写死

##### 写死

-   width:100px 

##### 不写死

-   width: 50% 
-   max-width: 100px 
-   width: 30vw   （屏幕宽度的百分之30）
-   min-width: 80% 
-   特点：不使用 px，或者加 min max 前缀

>   css 最忌讳把宽高写死

​	

#### 技术总结

##### 左右布局

+   表示两栏布局-贴左、贴右：可通过以下两句中的任意一句来实现
+   margin-xxx:  auto 更灵活

```html
<style>
  .header {
    display: flex;
    border:1px solid black;
    /*justify-content: space-between; ---------------二者任选其一----------------------*/
  }
  ul {
    /* margin-left: auto; -------------------------二者任选其一【推荐】------------------*/
    display: flex;
    border: 1px solid green;
  }
  ul > li {
    border: 1px solid red;
  }
</style>

<header class="header">
  <div class="logo">
    <img alt="" src="./logo.png">
  </div>
  <ul>
    <li>首页</li>
    <li>课程</li>
    <li>优惠</li>
    <li>关于</li>
  </ul>
</header>
```



##### 产品展示格子布局

###### 失败方案

```html
<div class="imageList">
  <div class="image"></div>
  <div class="image"></div>
  <div class="image"></div>
  <div class="image"></div>
  <div class="image"></div>
  <div class="image"></div>
  <div class="image"></div>
</div>
```

```css
.imageList{
  border: 1px solid red;
  width: 800px;
  margin-right: auto;
  margin-left: auto;
  margin-top: 10px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;  /* 会导致产品不足数的行 --> 错位 */
}
.image{
  width: 191px;
  height: 191px;
  background-color: #ccc;
  margin-bottom: 10px;
  border: 1px solid green;
}
```

<img src="https://i.loli.net/2020/07/23/Su3ExfocXphtwbr.png" alt="image-20200723171736064" style="zoom:50%;" />

​	



###### 负margin方案

```html
<div class="imageList">
  <!-- 可以命名为 inner 或者 wrapper -->
  <div class="x">
    <div class="image"></div>
    <div class="image"></div>
    <div class="image"></div>
    <div class="image"></div>
    <div class="image"></div>
    <div class="image"></div>
    <div class="image"></div>
  </div>
</div>
```

```css
.imageList {
  outline: 1px solid red;   /* 注意：边框去掉或者放在外面，否则占据宽度 */
  width: 800px;
  margin-right: auto;
  margin-left: auto;
  margin-top: 10px;
}
.imageList > .x {
  display: flex;
  flex-wrap: wrap;
  margin-right: -12px;    /* 负margin */
}
.image {
  width: 191px;
  height: 191px;
  background-color: #ccc;
  margin-bottom: 10px;
  border: 1px solid green;
  margin-right: 12px;  /* 每个的间距 */
}
```

![image-20200723172847224](https://i.loli.net/2020/07/23/Egd4TOSG1ce2o7q.png)









































