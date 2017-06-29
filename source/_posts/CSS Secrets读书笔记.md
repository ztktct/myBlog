---
title: CSS Secrets读书笔记
date: 2017/06/16 23:02
tags: [css]
categories: [css]
---
最近在看`CSS Secrets`这本书，真心觉得不错，书中的许多建议和技巧都让我对css有了新的认识。便借着博客将其中一些知识点记录下来，当做笔记，可以在日常工作、学习中常看看！
<!--more-->

## CSS编码技巧
### currentColor
`currentColor`算是css中的一个变量，它代表的不是某个具体的颜色，而是始终等于当前元素对应的`color`的值，`currentColor`本身是很多css颜色属性的初始值，像是`border-color`、`outline-color`、`text-shadow`等等。
{% jsfiddle v87gLmn2 html,css,result light 100% 200 %}

### inherit
以前虽然知道有这个关键字，但是没怎么用过，看到书上所讲的，突然发现一个困扰我许久的问题解决了：写CSS的时候,页面上经常有a标签，但是a标签默认是有颜色的，一般是蓝色，然是我又不想要他原来的颜色，也不想把颜色写死，这时候就可以考虑`inherit`这个关键词了。
``` css
  a{ color: inherit; }
```
{% jsfiddle uxr8t2q4 html,css,result light 100% 300 %}
还有伪元素写的小三角之类的颜色也直接设为`inherit`，这样就能自动继承背景和边框的样式：
{% jsfiddle fdy6vz0e html,css,result light 100% 300 %}

### 避免不必要的媒体查询
1. 尽量使用百分比%、em、rem、vw、vh、vmin、vmax代替固定长度
2. 不要忘记为替换元素(如img、object、video、iframe等)添加`max-width:100%`
3. 加入背景需要完整的铺满一个容器，可以考虑`background-size`
4. 使用`column`多列文本布局时，指定`column-width`而不是`column-count`，这样在较小的屏幕上会自动单列布局

### 合理使用简写

## 背景与边框
### 半透明边框
有时候我们需要用到半透明边框，但常常与我们预期的不一样，如下面的div01,这是由于背景的工作原理导致的，默认背景是漫延到边框部分的，幸好css第三版中增加了一个新特性，`background-clip`，它是用来规定背景的绘制区域的，常用的有以下取值：
```css
  background-clip: border-box; // 默认,背景包含border、padding、content三个部分
  backgorund-clip: padding-box; // 背景只包括padding、content两个部分
  background-clip: content-box; // 背景只包含content部分
```
我们要想凸显半透明边框，可以采用`backgorund-clip: padding-box;`：
{% jsfiddle wub7bazs html,css,result light 100% 300 %}

### 多重边框
1. `box-shadow`,这个属性可以接收第四个参数，扩张半径，通过指定正直或负值，可以让**投影面积加大或减小**
2. `outline`: 这个属性一直被我忽略，原来他也可以用来描边，他接受的参数跟`border`属性一样，可以设置边框的style(虚线、实线等),而且还有`outline-offset`属性用来控制距位置
{% jsfiddle ycbnmk0h html,css,result light 100% 300 %}

