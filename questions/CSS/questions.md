---
title: CSS 问题
---


## 目录
- [盒子模型。](#盒子模型)
- [px，em和rem的区别。](#pxem和rem的区别)
- [vv和vh的区别。](#vv和vh的区别)
- [水平垂直居中的方法。](#水平垂直居中的方法)
- [如何解决浮动塌陷。](#如何解决浮动塌陷)
- [常见布局方式。](#常见布局方式)
- [三栏布局。](#三栏布局)
- [position有哪些值。](#position有哪些值)
- [display有哪些属性。](#display有哪些属性)
- [伪类和伪元素区别。](#伪类和伪元素区别)
- [选择器优先级。](#选择器优先级)
- [怎么解决谷歌最小字符12像素的问题。](#怎么解决谷歌最小字符12像素的问题)
- [实现一个三角形。](#实现一个三角形)



### 盒子模型。
主要包括包含：
- margin
- border
- padding
- content

标准盒子模型（content-box）：width和height值包含content的内容
IE盒子模型（border-box）： width和height包括了内容区域的content，padding，border

[[↑] 回到顶部](#目录)


### px，em和rem的区别。

px是像素，em是当前内文本的字体尺寸，rem是根em。

[[↑] 回到顶部](#目录)


### vv和vh的区别。

- 1vw等于视口宽度的1%
- 1vh等于视口高度1%

[[↑] 回到顶部](#目录)


### 水平垂直居中的方法。

- 父元素relative，子元素aboluste 50% 50% translate: transform(-50%, -50%)
- 弹性布局，设置主轴和交叉的对齐方式

[[↑] 回到顶部](#目录)


### 如何解决浮动塌陷。

- 父元素也变成BFC，接管自己宽高，overflow:hidden
- 加一个after,clear: both, content:"",display: block, height: 0; visibility:hidden

[[↑] 回到顶部](#目录)


### 常见布局方式。

布局是决定页面大致的框架结构，因此十分重要。常见的布局有:
- table，目前已经很少用了。
- 浮动布局float
    - 脱离文档流，但是不会脱离文本流。
    - 形成BFC，可以设置宽高。
    - 会尽量往上，往右（左）靠，如果无法满足，会往下掉（宽度不够时）
- inline-block，对内是块状，可以设置宽高，对外是文字排版，不会独占一行
    - 像文本一样排block元素
    - 没有清除浮动等问题
    - 需要处理间隙，文字间存在间隙
    （1）父元素字体设置为0，字元素字体设置回来
    （2）去除div与div之间的空白
- 响应式设计和布局
    - 设计层面：
        - 隐藏
        - 折行
        - 自适应空间
    - 方法：
        - rem，先设置html的font-size值，那么1rem等于font-size的值，那么可以在媒体查询里面，写入不同的font-size
        - viewport，视图窗口大小和移动端屏幕大小统一
        - media query，根据不同的（width）给予不同的样式
        - flexbox布局
    
    
[[↑] 回到顶部](#目录)


### 三栏布局。

通过float, margin进行布局
```css
<div class="container">
<div class="left">1</div>
<div class="middle">asd</div>
<div class="right">2</div>
</div>
.container {
    width: 800px;
    height: 600px;
}
.left {
    float: left;
    width: 200px;
    height: 100%;
    background: red;
}
.middle {
    float: left;
    height: 100%;
}
.right {
    float: right;
    width: 200px;
    height: 100%;
    background: gainsboro;
}
```

[[↑] 回到顶部](#目录)


### position有哪些值。

- static，文档流布局
- relative， 相对于元素本身偏移，不会改变本身占据的空间，后面的元素会按照偏移之前的位置进行布局
- absolute，脱离文档流，文本流，不会对其他元素造成影响，找到父元素最近的relative（absolute）或一直找到body进行偏移
- fixed，脱离文档流，文本流，相对可视区域固定


[[↑] 回到顶部](#目录)


### display有哪些属性。

- table, table-row, table-cell，表格布局的一种
- flex，弹性布局
- block
- inline-block
- inline


[[↑] 回到顶部](#目录)


### 伪类和伪元素区别。


- 伪类表示状态，hover，focus，first-child，
- 伪元素是真的有元素，before，after


[[↑] 回到顶部](#目录)


### 选择器优先级。


- 计算权重，不会进位，ID100，类和伪类10，元素选择器和伪元素选择器为1。
- ！Important，优先级大于内联 
- 内联样式
- 后写的优先级高

[[↑] 回到顶部](#目录)


### 选择器优先级。

采用缩放的方式，scale(x, y)

[[↑] 回到顶部](#目录)


### 实现一个三角形。

```css
height: 0;
width: 0;
border: 20px solid;
border-color: transparent transparent transparent blue;
```

[[↑] 回到顶部](#目录)
