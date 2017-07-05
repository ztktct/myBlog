---
title: HTML5新特性学习系列——Notification
date: 2017/07/05 23:50
tags: [HTML5,javascript]
categories: [HTML5]
---
在登录网页版微信后，往往有新消息的时候，会从右上角出现提示，很好奇是怎么实现的，现在就来了解一下吧！
<!--more-->

## Notification API
该API用于向用户配置和显示桌面通知。

### 构造方法
```javascript
  let notification = new Notification(title, options);
```
#### 参数
* title: 一定会被显示的通知标题
* options: 用于配置通知
    * dir: 文字的方向；它的值可以是 auto（自动）, ltr（从左到右）, or rtl（从右到左）
    * lang: 指定通知中所使用的语言。这个字符串必须在 [BCP 47 language tag](https://tools.ietf.org/html/bcp47) 文档中是有效的。
    * body: 通知中额外显示的字符串。
    * tag: 赋予通知一个ID，以便在必要的时候对通知进行刷新、替换或移除。
    * icon: 一个图片的URL，将被用于显示通知的图标。

### 属性
#### 静态属性
##### Notification.permission(只读)
一个用于表明当前通知显示授权状态的字符串。可能的值包括：denied (用户拒绝了通知的显示), granted (用户允许了通知的显示), or default (因为不知道用户的选择，所以浏览器的行为与 denied 时相同).

#### 实例属性(全部只读)
* `notification.title`：在构造方法中指定的 title 参数。
* `notification.dir`：通知的文本显示方向。在构造方法的 options 中指定。
* `notification.lang`：通知的语言。在构造方法的 options 中指定。
* `notification.body`: 通知的文本内容。在构造方法的 options 中指定。
* `notification.tag`: 通知的 ID。在构造方法的 options 中指定。
* `notification.icon`: 通知的图标图片的 URL 地址。在构造方法的 options 中指定。

#### 实例事件
Notification 对象继承自 EventTarget 接口，所以可以用`addEventListener`来监听事件，`removeEventListener`来解绑事件，当然也可以直接用`on+事件名`来绑定事件。
* `click`: 当用户点击通知时被触发
* `show`: 当通知显示的时候被触发
* `error`: 每当通知遇到错误时被触发
* `close`: 当用户关闭通知时被触发

### 方法
#### 静态方法
`Notification.requestPermission()`：用于当前页面想用户申请显示通知的权限。这个方法只能被用户行为调用（比如：onclick 事件），并且不能被其他的方式调用。该方法接受一个回调方法，传入的参数是用户选择的是否允许通知，可能的值包括：`denied` , `granted` , or `default` .

#### 实例方法
`notification.close()`: 用于关闭通知。

## demo
{% jsfiddle omv0hh8h js,html,result dark 100% 800 %}

## 参考资料
* [notification - Web API 接口| MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/notification)
* [what we can do](https://whatwebcando.today/local-notifications.html)
