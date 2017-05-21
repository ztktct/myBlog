---
title: MobX学习笔记(持续学习中～)
date: 2017/05/07 22:30
tags: [MobX, mobx-react, react, javascript]
categories: [javascript, react]
---

最近公司项目用上了Mobx，就抽空来学习下吧～～
<!-- more -->
## MobX

### MboX使用
使用 MobX 将一个应用变成响应式的可归纳为以下三个步骤：
1. 定义状态并使其可观察
2. 创建视图以响应状态的变化
3. 更改状态

### MobX常用API
* @observable 使得数据可观察（extendObservable(this, {})语法糖）
-- extendObservable(target, props) 可以为一个对象引入新的可观察变量
* @computed 在相关数据发生变化时自动更新值（getter）,应该仅仅是返回一个值
* @action 建议在任何改变状态或具有副作用的函数上使用,(严格模式下必须使用)
* runInAction 对于只执行一次的行为 runInAction(name?, fn, scope?)
* autorun 会追踪副作用运行时使用的可观察变量,其返回一个销毁函数以取消副作用
* toJS 将一个（@observable）的对象转化为 javascript 结构

### mobx-react常用API
* Provider 可以通过React的上下文机制，将 store 传给子组件
* @inject 与 Provider 结合使用的部分。用于将store中的部分状态，通过上下文的形式注入给子组件  
* @observer 把无状态组件变成响应式组件 @inject("store1", "store2") @observer
* componentWillReact 新的自定义生命周期钩子，当一个组件准备重新渲染时将会被触发

### 参考资料
* [官方文档](https://mobx.js.org/index.html)
* [官方文档(中文版)](http://cn.mobx.js.org/)
