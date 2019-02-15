---
title: DOM及事件
date: 2018-10-07 10:03:16
tags:
- JS
category:
- JS
---
> 将HTML, SVG 或 XML 文档建模为对象并不是Javascript的一部分。DOM模型用一个逻辑树来表示一个文档，树的每个分支的终点都是一个节点(node)， 每个节点都包含着对象(objects)。DOM的方法(methods)让你可以用特定方式操作这棵树，用这些方法你可以改变文档的结构、样式或内容。

<br>
### 1. DOM简介
DOM是HTML和XML文档的编程接口。它提供了对文档的结构化的表述，并定义了一种方式可以使从程序中对该结构进行访问。DOM将文档解析为一个由节点和对象(属性和方法)组成的结构集合。DOM是web页面的完全的面向对象表述，它能够使用如Javascript等脚本语言进行修改。
<br>

#### 1.1 DOM API
*访问DOM*: 使用`document`或`window`元素的API来操作文档本身或获取文档的子类(Web页面的各种元素)。

- `window`对象表示浏览器中的内容;
- `document`对象是文档本身的根节点;
- `element`继承了通用的`node`接口。

```
  document.getElementById(id)           返回一个有特定id的元素
  document.getElementsByTagName(name)   返回一个有特定tagname的子元素列表
  document.createElement(name)          创建新的元素
  document.createTextNode("text")       创建文本节点
  parentNode.appendChild(node)          将node设置为parentNode的一个新的子元素（放在最后一个节点）
  parentNode.removeChild(node)          移除节点
  parentNode.childNodes                 获得parentNode的孩子节点列表
  element.innerHTML
  element.style.left                    设置HTML元素的style样式
  element.setAttribute()                设置HTML元素的属性
  element.getAttribute()                获取HTML元素的属性值
  element.addEventListener()            指定元素的事件处理程序
  // addEventListen接受3个参数：事件名，处理函数，布尔值（true-捕获阶段处理，false-冒泡阶段处理）
  element.removeEventListener()         删除元素的事件处理程序
  window.content
  window.onload
  window.dump()
  window.scrollTo()
```
<br>

#### 1.2 DOM的数据类型
一般，`element(s)`指代节点，`nodeList(s)或element(s)`指代节点数组，`attribute(s)`指代属性节点。

DOM的数据类型 | |
---|:--:|---:
document	      | 这个对象是`root document`对象本身。
elemnt          | `document.createElement()`方法会返回一个`node`的对象引用，该element对象实现了DOM element接口以及更基本的Node接口
nodeList        | nodeList是一个元素的数组，从`document.getElementsByTagName()`方法返回的类型
attribute       | attribute通过`createAttribute()`方法返回，它是一个为属性暴露出专门接口的对象一样。DOM中属性也是节点。
namedNodeMap    | nameNodeMap和数组类似
<br>

### 2. DOM事件模型
#### 2.1 DOM事件流
事件流包括三个阶段：事件捕获阶段 ==> 处于目标阶段 ==> 事件冒泡阶段。
<img src="2.png" style="padding:20px">

- *事件冒泡*
IE的事件流叫事件冒泡，即事件开始时由最具体的元素接受，然后逐级向上传播到较为不具体的节点。

- *事件捕获*
Netscape团队提出的另一种事件流叫事件捕获，指不太具体的节点应该最早接收到事件，而最具体的节点最后接收到时间。

- stopPropagation()
不再派发事件。终止事件在传播过程中的捕获、目标处理、或冒泡阶段的进一步传播。调用该方法后，该节点上处理该事件的处理程序将被调用，事件不再被分配到其他节点。**在事件传播的任何阶段都可以调用。**
<br>

#### 2.2 注册事件监听器
- EventTarget.addEventListener
```
  // IE 6-8不支持这一方法，但提供了element.attachEvent用来替代。
  myButton.addEventListener('click', function(){alert('hello')}, false)
```

- HTML属性
```
  <button onclick="alert('hello')">
```

- DOM元素属性
```
  // event可以在回调函数内被访问到，做为第一个参数的事件对象。
  myButton.onclick = function(event){
    event.stopPropagation();
    alert('hello')
  }
```
<br>

#### 2.3 Event对象
属性 | |
---|:--:|---:
Event.bubbles	          | Boolean，表示该事件是否在DOM中冒泡
Event.cancelable        | Boolean，表示事件是否可以取消
Event.currentTarget     | 当前注册事件的对象的引用，这个值在传递的途中进行改变
Event.defaultPrevented  | Boolean，是否已经执行过event.preventDefault
Event.target            | 对事件起源目标的引用
Event.timeStamp         | 事件创建时的时间戳
Event.type              | 事件的类型
**方法**                |
Event.preventDefault    | 取消事件默认事件
Event.stopImmediatePropagation  | 
Event.stopPropagation   | 停止事件冒泡
<br>

#### 2.4 事件类型
事件 | |
---|:--:|---:
焦点事件       | focus, blur
表单事件       | reset, submit
Websocket事件 | open, message, error, close
打印时间       | beforeprint, afterprint
视图事件       | fullscreenchange, fullscreenerror, resize, scroll
剪贴板事件     | cut, copy, paster
键盘事件       | keydown, keypress, keyup
鼠标事件       | mouseenter, mouseover, mousemove, mousedown, mouseup, click, dbclick, contextmenu(右击), wheel(滚轮滚动),mouseleave, mouseout, select(文本被选中)
拖放事件       | dragstart, drag, dragend, dragenter, dragover, dragleave, drop
[更多事件查看文档](https://developer.mozilla.org/zh-CN/docs/Web/Events)

<br>
#### 2.5 事件代理
事件代理用到事件冒泡和目标元素，把事件处理器添加到父元素，等待子元素事件冒泡，并且父元素能够通过target (IE为scrElement) 判断是哪个子元素，从而做相应处理。

*好处*: ①将多个事件处理器减少到一个，因为事件处理器要驻留内存，这样就提高了性能。②DOM更新无需重新绑定时间处理器，直接修改事件处理函数即可。

```
<ul id="color-list">
  <li>red</li>
  <li>orange</li>
</ul>
<script>
  (function(){
    var colorList = document.getElementById('color-list');
    colorList.addEventListener('click', showColor, false);

    function showColor(e) {
      e = e || window.event;
      var targetElement = e.target || e.srcElement;
      if (targetElement.nodeName.toLowerCase() === 'li'){
        alert(targetElement.innerHTML)
      }
    }
  })();
</script>
```
<br>

### 3. 使用选择器定位DOM元素
#### 3.1 原生选择器
- *querySelector*
返回节点子树内与之相匹配的第一个`element`节点。如果没有匹配的节点，则返回`null`。选择器方法接受一个或多个逗号分隔，来确定应该返回的元素。
```
  // 选择文档中class是warning或note的p元素
  var special = document.querySelectorAll("p.warning, p.node")
```

- *querySelectorAll*
返回一个`NodeList`，包含节点子树内所有与之相匹配的Element节点，如果没有相匹配的，返回一个空节点列表。
```
  // 文档中id是main, 或basic, 或exclamation的所有元素中的第一个元素
  var el = document.querySelector("#main, #basic, #exclamation")
```
<br>

#### 3.2 jQuery选择器
<br>

### 4. DOM中的空白符
有些空白符会自成一个文本节点。有些空白符会与其他文本节点合成为一个文本节点。
```
  <body>
    <h1>Header</h1>
    <p>
      Paragraph
    </p>
  </body>
```
<img src="1.png" style="padding-top:20px">

### 参考资料
- [MDN-文件对象模型](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model)
- [理解Dom事件机制](https://segmentfault.com/a/1190000011951192)