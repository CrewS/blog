---
title: Vue数据绑定
date: 2018-11-14 15:47:27
tags:
---

### 1. MVVM
MVVM拆开来即为Model-View-ViewModel，由View，ViewModel，Model三部分组成。它目的在于更清楚地将用户界面（UI)的开发与应用程序中业务逻辑和行为的开发区分开来。

<img src="1.png" style="max-width: 400px">
**Model**
数据层
是对现实世界中事物的抽象结果，就是建模。

**View**
用户操作界面
负责将数据模型转化为UI展现出来。

**ViewModal**
业务逻辑层
View需要什么数据，ViewModel要提供这个数据；View有哪些些操作，ViewModel就要响应这些操作


<br/>
### 2. 数据驱动
对于 View 来说，如果封装得好，一个UI组件能很方便地给大家复用。对于 Model 来说，它其实是用来存储业务的数据的，如果做得好，它也可以方便地复用。那么，ViewModel有多少可以复用？我们写完了一个 Vue实例之后，可以很方便地复用它吗？结论是：*非常难复用*。

ViewModel做的什么？就是写那些不能复用的业务代码。当交互复杂，一个数据的改变引起多处DOM的修改。ViewModel将变得*十分臃肿*。
```
  myKey = 1;
  document.getElementById('myKey').innerHTML = myKey;
  document.getElementById('abouMyKey').innerHTML = myKey;
  // ... 更多复杂的交互
```

怎么简化ViewModel？*数据驱动: 数据的变更触发DOM的变化*。
我们希望，只执行`this.$data.myKey = 1`就可以触发与`myKey`相关的UI更新。
```
  this.$data.myKey = 1
```

> 所谓数据驱动，是指视图是由数据驱动生成的，我们对视图的修改，不会直接操作 DOM，而是通过修改数据。它相比我们传统的前端开发，如使用 jQuery 等前端库直接修改 DOM，大大简化了代码量。特别是当交互复杂的时候，只关心数据的修改会让代码的逻辑变的非常清晰，因为 DOM 变成了数据的映射，我们所有的逻辑都是对数据的修改，而不用碰触 DOM，这样的代码非常利于维护。


<br/>
### 3. 数据劫持
怎么做到，只执行`this.$data.myKey = 1`就可以触发与`myKey`相关的UI更新？
1. 通过Object.defineProperty()来劫持对象属性的setter操作
2. 

<br/>

#### 3.1 Object.defineProperty
利用 Object.defineProperty 给数据添加了 getter 和 setter，目的就是为了在我们访问数据以及写数据的时候能自动执行一些逻辑：getter 做的事情是依赖收集，setter 做的事情是派发更新

具体查看[文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

在我们访问一个属性的时候会触发getter方法，当我们对一个属性做修改的时候会触发setter方法。
```
Object.defineProperty(obj, key, {
  enumerable: true,
  configurable: true,
  get: function() {
      console.log('get');        
  },
  set: function(newVal) {
      console.log('数据变化了');
      updateUI();
  }
});
```


get 和 set，get 是一个给属性提供的 getter 方法，当我们访问了该属性的时候会触发 getter 方法；set 是一个给属性提供的 setter 方法，当我们对该属性做修改的时候会触发 setter 方法。


### 2. 依赖收集
Vue采用数据劫持&发布-订阅模式的方式，通过Object.defineProperty()方法来劫持各属性的getter, setter，并在数据发生变动时通知ViewModel重新编译模板。



getter 收集依赖
这时候会做一些逻辑判断（保证同一数据不会被添加多次）后执行 dep.addSub(this)，那么就会执行 this.subs.push(sub)，也就是说把当前的 watcher 订阅到这个数据持有的 dep 的 subs 中，这个目的是为后续数据变化时候能通知到哪些 subs 做准备。

清空依赖
考虑到 Vue 是数据驱动的，所以每次数据变化都会重新 render，那么 vm._render() 方法又会再次执行，并再次触发数据的 getters

考虑到一种场景，我们的模板会根据 v-if 去渲染不同子模板 a 和 b，当我们满足某种条件的时候渲染 a 的时候，会访问到 a 中的数据，这时候我们对 a 使用的数据添加了 getter，做了依赖收集，那么当我们去修改 a 的数据的时候，理应通知到这些订阅者。那么如果我们一旦改变了条件渲染了 b 模板，又会对 b 使用的数据添加了 getter，如果我们没有依赖移除的过程，那么这时候我去修改 a 模板的数据，会通知 a 数据的订阅的回调，这显然是有浪费的。

因此 Vue 设计了在每次添加完新的订阅，会移除掉旧的订阅，这样就保证了在我们刚才的场景中，如果渲染 b 模板的时候去修改 a 模板的数据，a 数据订阅回调已经被移除了，所以不会有任何浪费，真的是非常赞叹 Vue 对一些细节上的处理。

<img src="2.png">
<img src="3.png">
<img src="4.png">

<br/>
### 参考资料
- [简单理解MVVM--实现Vue的MVVM模式](https://zhuanlan.zhihu.com/p/38296857)
- [被误解的 MVC](https://blog.devtang.com/2015/11/02/mvc-and-mvvm/)
- [Vue.js中的MVVM](https://juejin.im/post/5b2f0769e51d45589f46949e)
- [Vue.js的响应式系统原理](https://juejin.im/post/5b82b174518825431079d473)
- [Vue技术揭秘](https://ustbhuangyi.github.io/vue-analysis/data-driven/)
