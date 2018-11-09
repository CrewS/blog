---
title: CSS常用实例
date: 2018-11-06 17:57:46
tags: CSS
---

#### 1. 全屏背景
```
body{
  background-image: url(1.png);         // 背景图路径
  background-position: center center;   // 背景图垂直水平居中
  background-repeat: no-repeat;         // 不平铺
  background-attachment: fixed;         // 当内容高度大于图片高度时，背景图像的位置相对于viewport固定
  background-size: cover;               // 背景图基于容器大小伸缩
  background-color: #fff;               // 背景图加载过程中显示的背景色
}
```
<br/>

#### 2. 单行溢出
```
a{
  max-width: 100px;        // 固定宽度
  white-space: nowrap;     // 禁止换行
  overflow: hidden         // 隐藏溢出文本
  text-overflow: ellipsis; // 超出部分显示...
}
```
<br/>

#### 3. 多行溢出

<br/>
#### 4. 自动换行
`word-wrap`可以控制换行，当取值`break-word`时将强制换行，中英文文本都无任何问题，但是对长串的英文不起作用。也就是说`break-word`是用来断词而不是断字符。而`word-break`取值为`break-all`时，可允许非亚洲语言文本的任意字符断开。

一般来说，使用`word-wrap:break-word`声明可以确保所有文本正常显示。但在Firefox浏览器上，长串英文会出现问题(不换行)。为了解决长串英文问题，一般将`word-wrap: break-word; word-break: break-all`一起使用。但是这样又造成了一个新的问题，会导致普通英文语句中的单词断行影响阅读。

综上所述，主要问题是长串英文不换行和英文单词会被断开。两者选一，应该使用`word-wrap: break-word; overflow:hidden`结合，而不是`word-wrap: break-word; word-break:break-all`结合。
```
element{
  overflow: hidden;
  word-wrap: break-word;
}
```

<br/>
#### 5. flex布局
- 网格布局，平均分布
{% raw %}
<style>
.box{
  width:100%;
  height:50px;
  display:flex;
  margin-top:10px;
  background:#f6f6f6;
}
.item{
  background:#fff;
  margin:10px;
  flex:1;
}
</style>
<div class="box">
  <div class="item"></div>
  <div class="item"></div>
  <div class="item"></div>
</div>
{% endraw %}
```
.box{
  display: flex;
}
.item{
  flex: 1;
}
<div class="box">
  <div class="item"></div>
  <div class="item"></div>
  <div class="item"></div>
</div>
```
<br/>

- 百分比布局
{% raw %}
<style>
.box{
  width:100%;
  height:50px;
  display:flex;
  margin-top:10px;
  background:#f6f6f6;
  text-align:center;
}
.item1{
  background:#fff;
  margin:10px;
  flex:0 0 25%;
}
.item2{
  background:#fff;
  margin:10px;
  flex:1;
}
</style>
<div class="box">
  <div class="item1">1/4</div>
  <div class="item2">auto</div>
</div>
{% endraw %}
```
.box{
  display:flex;
}
.item1{
  flex:0 0 25%;
}
.item2{
  flex:1;
}
```
<br/>

- 圣杯布局： 页面从上到下，分成header, body, footer。其body从左到右又分为导航，主栏，副栏。
固定的底栏：页面内容太少，无法占满一屏的高度，底栏就会抬高到页面的中间，这时可以采用该布局。
{% raw %}
<style>
.box1{
  display: flex;
  min-height: 200px;
  flex-direction: column;
  margin-top: 10px;
  background:#f6f6f6;
  text-align: center;
  padding:10px;
}
.box1 div{
  background: #fff;
}
.box1 .box1-container{
  background: #f6f6f6;
}
.box1 .box1-header{
  flex:0 0 20px;
}
.box1 .box1-footer{
  flex:0 0 20px;
}
.box1-container{
  flex: 1;
  display:flex;
  margin: 10px 0;
}
.box1-aside{
  order: -1;
  flex: 0 0 50px;
  margin-right: 10px;
}
.box1-right-aside{
  order: 1;
  flex: 0 0 100px;
  margin-left: 10px;
}
.box1-content{
  flex: 1;
}

</style>
<div class="box1">
  <div class="box1-header">header</div>
  <div class="box1-container">
    <div class="box1-aside">aside</div>
    <div class="box1-content">content</div>
    <div class="box1-right-aside">right-aside</div>
  </div>
  </section>
  <div class="box1-footer">footer</div>
</div>
{% endraw %}
```
.box{
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}
.header, .footer{
  flex: 1;
}
.container{
  flex: 1;
  display: flex;
}
.aside, .right-aside{
  flex: 0 0 100px;
}
.content{
  flex: 1;
}
<div class="box">
  <div class="header">header</div>
  <div class="container">
    <div class="aside">aside</div>
    <div class="content">content</div>
    <div class="right-aside">right-aside</div>
  </div>
  <div class="footer">footer</div>
</div>
```

<br/>
- 流式布局: 每行的项目数固定，会自动分行
{% raw %}
<style>
.parent {
  width: 100%;
  height: 150px;
  background-color: #f6f6f6;
  display: flex;
  flex-flow: row wrap;
  padding: 10px;
}

.child {
  box-sizing: border-box;
  background-color: #fff;
  flex: 0 0 30%;
  height: 50px;
  margin: 10px 1.5% 0;
}
</style>
<div class="parent">
  <div class="child"></div>
  <div class="child"></div>
  <div class="child"></div>
  <div class="child"></div>
  <div class="child"></div>
</div>
{% endraw %}

  ```
  .parent {
    width: 100%;
    height: 150px;
    display: flex;
    flex-flow: row wrap;
  }

  .child {
    box-sizing: border-box;
    flex: 0 0 30%;
    height: 50px;
    margin: 10px 1.5% 0;
  }
  ```

<br/>

#### 6. 纹理背景
更多详情查看 https://leaverou.github.io/css3patterns/
{% raw %}
<style>
.veins div{
  width: 250px;
  height: 250px;
  display: inline-block;
}
.veins1{
  background:
  radial-gradient(black 15%, transparent 16%) 0 0,
  radial-gradient(black 15%, transparent 16%) 8px 8px,
  radial-gradient(rgba(255,255,255,.1) 15%, transparent 20%) 0 1px,
  radial-gradient(rgba(255,255,255,.1) 15%, transparent 20%) 8px 9px;
  background-color:#282828;
  background-size:16px 16px;
}
.veins2{
  background-color: #6d695c;
  background-image:
  repeating-linear-gradient(120deg, rgba(255,255,255,.1), rgba(255,255,255,.1) 1px, transparent 1px, transparent 60px),
  repeating-linear-gradient(60deg, rgba(255,255,255,.1), rgba(255,255,255,.1) 1px, transparent 1px, transparent 60px),
  linear-gradient(60deg, rgba(0,0,0,.1) 25%, transparent 25%, transparent 75%, rgba(0,0,0,.1) 75%, rgba(0,0,0,.1)),
  linear-gradient(120deg, rgba(0,0,0,.1) 25%, transparent 25%, transparent 75%, rgba(0,0,0,.1) 75%, rgba(0,0,0,.1));
  background-size: 70px 120px;
}
.veins3{
  background-color:silver;
  background-image:
  radial-gradient(circle at 100% 150%, silver 24%, white 25%, white 28%, silver 29%, silver 36%, white 36%, white 40%, transparent 40%, transparent),
  radial-gradient(circle at 0    150%, silver 24%, white 25%, white 28%, silver 29%, silver 36%, white 36%, white 40%, transparent 40%, transparent),
  radial-gradient(circle at 50%  100%, white 10%, silver 11%, silver 23%, white 24%, white 30%, silver 31%, silver 43%, white 44%, white 50%, silver 51%, silver 63%, white 64%, white 71%, transparent 71%, transparent),
  radial-gradient(circle at 100% 50%, white 5%, silver 6%, silver 15%, white 16%, white 20%, silver 21%, silver 30%, white 31%, white 35%, silver 36%, silver 45%, white 46%, white 49%, transparent 50%, transparent),
  radial-gradient(circle at 0    50%, white 5%, silver 6%, silver 15%, white 16%, white 20%, silver 21%, silver 30%, white 31%, white 35%, silver 36%, silver 45%, white 46%, white 49%, transparent 50%, transparent);
  background-size: 100px 50px;
}
.veins4{
  background-color:#556;
  background-image: linear-gradient(30deg, #445 12%, transparent 12.5%, transparent 87%, #445 87.5%, #445),
  linear-gradient(150deg, #445 12%, transparent 12.5%, transparent 87%, #445 87.5%, #445),
  linear-gradient(30deg, #445 12%, transparent 12.5%, transparent 87%, #445 87.5%, #445),
  linear-gradient(150deg, #445 12%, transparent 12.5%, transparent 87%, #445 87.5%, #445),
  linear-gradient(60deg, #99a 25%, transparent 25.5%, transparent 75%, #99a 75%, #99a),
  linear-gradient(60deg, #99a 25%, transparent 25.5%, transparent 75%, #99a 75%, #99a);
  background-size:80px 140px;
  background-position: 0 0, 0 0, 40px 70px, 40px 70px, 0 0, 40px 70px;
}
.veins5{
  background-color:#269;
  background-image: linear-gradient(white 2px, transparent 2px),
  linear-gradient(90deg, white 2px, transparent 2px),
  linear-gradient(rgba(255,255,255,.3) 1px, transparent 1px),
  linear-gradient(90deg, rgba(255,255,255,.3) 1px, transparent 1px);
  background-size: 100px 100px, 100px 100px, 20px 20px, 20px 20px;
  background-position:-2px -2px, -2px -2px, -1px -1px, -1px -1px;
}
.veins6{
  background-color:#001;
  background-image: radial-gradient(white 15%, transparent 16%),
  radial-gradient(white 15%, transparent 16%);
  background-size: 60px 60px;
  background-position: 0 0, 30px 30px;
}
.veins7{
  background-color: #fff;
  background-image:
  linear-gradient(90deg, transparent 79px, #abced4 79px, #abced4 81px, transparent 81px),
  linear-gradient(#eee .1em, transparent .1em);
  background-size: 100% 1.2em;
}
.veins8{
  background-color:white;
  background-image: linear-gradient(90deg, rgba(200,0,0,.5) 50%, transparent 50%),
  linear-gradient(rgba(200,0,0,.5) 50%, transparent 50%);
  background-size:50px 50px;
}
.veins9{
  background-color: gray;
  background-image: linear-gradient(90deg, transparent 50%, rgba(255,255,255,.5) 50%);
  background-size: 50px 50px;
}
</style>
<div class="veins">
  <div class="veins1"></div>
  <div class="veins2"></div>
  <div class="veins3"></div>
  <div class="veins4"></div>
  <div class="veins5"></div>
  <div class="veins6"></div>
  <div class="veins7"></div>
  <div class="veins8"></div>
  <div class="veins9"></div>
</div>
{% endraw %}

<br/>



#### 参考资料

- [CSS3 Patterns Gallery 纹路背景](https://leaverou.github.io/css3patterns/)
- [Flex 布局教程：实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)
- [多行溢出](https://www.jianshu.com/p/d2be62a507b8)
- 《图解CSS3·核心技术与案例实践》 大漠[著]