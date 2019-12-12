---
title: CSS 盒模型与外边距合并
date: 2019-12-12 10:49:30
tags: CSS
---

## 什么是 CSS 盒模型

首先看看 W3C 给出的定义：

The CSS box model describes the rectangular boxes that are generated for elements in the document tree and laid out according to the visual formatting model.

盒模型定义了在文档树中的矩形区域，它基于视觉格式化模型进行布局。

通俗来说，就是给页面元素定义的一些「盒子」，这个「盒子」有它自己的行为。

## 盒模型介绍

进行页面布局的时候，浏览器引擎将元素基于盒模型进行处理。CSS 会定义元素的大小、位置、及其他属性。

盒模型由 `content area` ，和可选的 `padding area` `border area` `margin area` 构成。各自的边缘决定每个区域的范围。

下图描述的各个区域及其边缘。

<img src="http://assets.wzbspace.top/img/20191212145430.png" alt="01912121454"  />

我们也经常能在 `Chrome Devtools - Styles` 下，见到下它。

<img src="http://assets.wzbspace.top/img/box-model-dsfajk.png" alt="box-model" style="zoom: 50%;"/>

## 两种盒子模型

设置元素的 `box-sizing` 属性，能修改元素的高宽计算方式。

`box-sizing` 属性有两个可选值：

+ `content-box` (CSS 标准定义的默认值)
+ `border-box`

### W3C 标准盒子模型

当 `box-sizing` 为默认值 `content-box` 时，元素的高宽与内容和区域高宽相等，为标准盒子模型。

即
+ `height = content height`
+ `width = content height`

<img src="http://assets.wzbspace.top/img/content-box-css-box-model.png" alt="border-box" style="zoom: 50%;"/>

### IE 怪异盒子模型

当 `box-sizing` 值为 `box-sizing` 时，元素的高宽计算方式如下：

+ `height = border-top + padding-top + content height + padding-bottom + border-bottom`
+ `width = border-left + padding-left + content-width + padding-right + border-right`

<img src="http://assets.wzbspace.top/img/border-box-css-box.png" alt="border-box" style="zoom: 50%;"/>

### DOCTYPE

我们在编写页面时，声明 HTML 文档的 `DOCTYPE` 能让浏览器对盒模型对处理行为统一，保证兼容性。

## 外边距合并

**毗邻**块级元素的上外边距和下外边距有时会合并（或折叠）为一个外边距，其大小取其中的最大者，这种行为称为外边距折叠（margin collapsing），也作外边距合并。

我们讨论对外边距合并，都是指竖直方向上对合并，水平方向上不会发生外边距合并。

我们先来看两种常见情况：

### 兄弟盒子外边距合并

<img src="http://assets.wzbspace.top/img/siblings-margin-collapsing.png" alt="siblings-margin-collapsing" style="zoom: 100%;" />

可见，上下兄弟元素在竖直方向上发生了外边距折叠。

### 父子盒子外边距合并

父子盒子的外边距合并，需要子元素与父元素的上下 `margin edge` 之间不能有间隙。

<img src="http://assets.wzbspace.top/img/container-box-margin-collpasing.png" alt="contain-box-margin-collpasing-code" style="zoom: 50%;" />

<img src="http://assets.wzbspace.top/img/Kapture 2019-12-12 at 16.18.53.gif" alt="contain-box-margin-collpasing" style="zoom: 40%;">

### 外边距合并发生的条件

那么，什么情况下会发生外边距合并呢？

以下是 W3C 标准文档中的描述

> 1. Margins between a floated box and any other box do not collapse (not even between a float and its in-flow children).
> 2. Margins of elements that establish new block formatting contexts (such as floats and elements with 'overflow' other than 'visible') do not collapse with their in-flow children.
> 3. Margins of absolutely positioned boxes do not collapse (not even with their in-flow children).
> 4. Margins of inline-block boxes do not collapse (not even with their in-flow children).
> 5. The bottom margin of an in-flow block-level element always collapses with the top margin of its next in-flow block-level sibling, unless that sibling has clearance.
> 6. The top margin of an in-flow block element collapses with its first in-flow block-level child's top margin if the element has no top border, no top padding, and the child has no clearance.
> 7. The bottom margin of an in-flow block box with a 'height' of 'auto' and a 'min-height' of zero collapses with its last in-flow block-level child's bottom margin if the box has no bottom padding and no bottom border and the child's bottom margin does not collapse with a top margin that has clearance.
> 8. A box's own margins collapse if the 'min-height' property is zero, and it has neither top or bottom borders nor top or bottom padding, and it has a 'height' of either 0 or 'auto', and it does not contain a line box, and all of its in-flow children's margins (if any) collapse.

简译过来为：

1. 浮动的盒子不会发生外边距合并
2. 触发 `BFC` 的盒子与子元素不会发生外边距合并
3. 设置绝对定位的盒子不会发生外边距合并
4. 行内块级元素不会发生外边距合并
5. 盒子的 `bottom margin` 与 下一个盒子的 `top margin` 总会合并，除非下一个盒子清除浮动。 
6. 如果父级元素没有设置 `top padding` `top border` 并且 首个子元素没有清除浮动的话，首个子元素的 `top margin` 会与父级元素的 `top margin` 合并。
7. 如果父级盒子元素 的 `height` 为 `auto` `min-height` 为 `0` ，且 `padding-bottom` 和 `border-bottom` 都为 `0`。父级盒子元素的 `bottom margin` 与 最后一个子元素(不能有清除浮动)的 `bottom margin` 发生外边距合并。
8. 如果盒子元素的 `min-height` 为 `0`，它没有上下 `border `与上下 `padding`，并且它的 `height` 为 `0` 或者 `auto`，并且不包含行内盒子，它的子盒子元素都会折叠。

### 合并边距大小的计算

发生了外边距合并，合并的边距的计算分方式如下：

+ 同为正值时, 取较大者为两者为间距
+ 一正一负时, 正负相加为间距, 若结果为负值, 则两者部分重合
+ 都为负值时, 两者重合, 且重合部分为绝对值大者

## 参考资料

+ [Box Model - W3C](https://www.w3.org/TR/CSS2/box.html)
+ [CSS Box Model - MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model)
+ [The IE box model and Doctype modes - listamatic](http://css.maxdesign.com.au/listamatic/about-boxmodel.htm)
+ [你真的了解盒模型么 - 大转转 FE](https://mp.weixin.qq.com/s/Z0L2geWYqZ7Kly-ivOBJsQ)