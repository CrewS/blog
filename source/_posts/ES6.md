---
title: ES6常用语法
date: 2018-10-15 17:49:23
tags:
- ES6
---
### 1. 块级作用域
> *块级声明* : 声明在指定块的作用域之外无法访问的变量。存在于函数内部和块`{}`中。一旦执行到块外会立即销毁。

- let
let可以把变量的作用域限制在当前代码块中，let声明不会被提升。

- const
const声明的常量必须进行初始化，值一旦设定不可更改(对象的值可修改)。

- 循环中的let
每次循环的时候let声明都会创建一个新变量i, 并将其初始化为i的当前值
```
  var a = [];
  for(let i = 0; i < 10; i++){
    a.push(function(){
      console.log(i)
    })
  }
  a.forEach(function(func){
    func(); // 0,1,2 ... 9
  })
```
<br>

### 2. 模板字面量
实现了多行字符串，将变量的值嵌入字符串，HTML转义(向HTML插入经过安全转换后的字符串)的能力
- 在反撇号中所有空白符都属于字符串的一部分.你可以显示地使用`\n`来指明换行。
```
  var a = `hello
  world`; 
  console.log(a.length) // 16 
```
- 占位符`${}` 中可以包含Javascript表达式
```
  var count = 2;
  var b = `hello, ${count}`
  var c = `hello, ${(count * 2).toFixed(2)}`
```
<br>

### 3. 函数
- 为形参提供默认值
```
function a(num = getValue(), timeout = 2000, callback = function(){}){}
a(20, null, function(){ console.log('1')})    // 不使用默认值
```
- 默认参数的临时死区
```
function getValue(value){
  return value + 5;
}

function add(first, second = getValue(first)){
  // 执行add()时，相当于执行以下代码来创建first和second参数值
  // let first = 1; 
  // let second = getValue(1) 
  return first + second;
} 

add(1)  // 7 
```

#### 3.1 不定参数(...)
不定参数可以让你指定多个独立的参数，通过整合后的数组来访问。
```
function checkArgs(...args){
  console.log(args.length);       // 2
  console.log(arguments.length);  // 2
}
checkArgs("a", "b")
// 使用限制：每个函数只能声明一个不定参数，且一定要放在所有参数的末尾
```

#### 3.2 展开运算符(...)
展开运算符可以让你指定一个数组，将它们打散后作为独立的参数传入函数。
```
let value = [1,2,3,4];
console.log(Math.max(...values));
// 等价于console.log(Math.max(1,2,3,4))
```

#### 3.3 箭头函数
箭头函数是一种使用箭头(=>)定义函数的新语法
- 箭头函数中的`this, super, arguments, new.target`的值由外围最近一层非箭头函数决定
- 箭头函数不能被用作构造函数，不能通过`new`调用
- 没有原型，不存在`prototype`这个属性
- 函数内部的`this`值不可被改变
- 不支持`arguments`对象

```
let a = () => 'Nicholas';
let b = value => value;
lect c = (num1, num2) => {
  return num1 + num2;
}
let d = id => ({ id: id, name: 'Temp'}) // 让箭头向外返回一个对象，应包裹在小括号里
```
<br>

### 4. 对象
#### 4.1 简写、可计算的属性名
```
var name = 'Mike';
var suffix = "name";
var person = {
  name,
  ['first' + suffix]: 'Zakas',
  sayName() {
    console.log(this.name)
  }
}
```

#### 4.2 Object.assign
接受任意数量的源对象，并按指定的顺序将属性复制到接收对象中。
```
var receiver = {};
Object.assign(receiver,{type: 'js', name: 'file.js'}, {type: 'css'});
console.log(receiver.type)  // 'css'
```

#### 4.3 super
简化原型访问的`super`引用，指向对象原型的指针，实际是`Object.getPrototypeOf(this)`的值。
```
  let person = {
    getGreeting(){ return 'hello ' }
  }
  let friend = {
    getGreeting() { return super.getGreeting() + 'friend' } // hello friend
  }
```
<br>

### 5. 解构
简化获取信息的过程，无需深入整个数据结构找到数据，编写代码为变量赋值。

- 对象解构
```
let node = {type: 'Identifier', name: 'foo'};
let { type, name } = node;

// 为非同名局部变量赋值
let { type: localType, name: localName } = node;
console.log(localType)   // 'Identifier'
```

- 数组解构
```
let colors = ["red", "green"];
let [firstColor, secondColor] = colors;
console.log(firstColor)   // "red"
```
<br>

### 6. 类
ES6使用`class`，取代需要`prototype`的操作。

#### 6.1 constructor()
用`constructor`方法名来定义构造函数
```
class Rectangle {
  constructor(length, width) {
    this.length = length;
    this.width = width;
  }
  getArea() {
    return this.length * this.width;
  }
}
```

#### 6.2 super()
`super()`方法可访问基类的构造函数。ES6要求，子类的构造函数必须执行依次super函数。
```
class Square extends Rectangle {
  constructor(length) {
    super(length, length)
  }
}

var square = new Square(3);
console.log(square.getArea()) // 9
```
<br>

### 7. 模块
模块是一种打包和封装功能的方式，模块的行为与脚本不同，模块不会将它的顶级变量、函数和类修改为全局作用域，而且`this`的值为`undefined`。

#### 7.1 export
将一部分已发布的代码暴露给其他模块
```
export var color = "red";
export function sum(num1, num2) {
  return num1 + num2;
}
export class Rectangle {
  constructor(height, width) {
    this.length = length;
    this.width = width;
  }
}

function multiply(num1, num2){
  return num1 * num2;
}

// 导出默认值
export default multiply;
```

#### 7.2 import
从模块中导出的功能可以通过`import`关键字在另一个模块中访问
```
import { identifier1, identifier2 } from "./example.js";
import * as example from "./example.js"
```

#### 7.3 加载模块
通过`<script type="module">`加载的模块文件默认具有defer属性，在文档完全被解析后，模块按照它们在文档中出现的顺序依次执行。
```
<script type="module" src="module.js"></script>
<script type="module">
  import { sum } from "./example.js"
  let result = sum(1, 2)
</script>
```
<br>

### 8. 其它
Set、Map、Symbol、Interator、Generator、Promise、Proxy、Reflection

#### 8.1 Set过滤重复值
对一个数组，进行复制并创建一个无重复元素的新数组。
```
  let set = new Set([1,2,3,3,4,5]);
      array = [...set];
  console.log(array);   // [1,2,3,4,5]
```

#### 8.2 Generator的异步应用
##### 8.2.1 协程
多个线程互相协作，完成异步任务。大致流程：①协程A开始执行。②协程A执行到一半，进入暂停，执行权转移到协程B。③(一段时间后)协程B交还执行权。④协程A恢复执行。

协程遇到yield命令就暂停，等到执行权返回，再从暂停的地方继续往后执行。*优点：写法非常像同步操作*。每次调用`next`方法，会返回一个对象，表示当前阶段的信息(`value`和`done`)。`value`是`yield`语句后表达式的值，`done`是一个布尔值，表示Generator函数是否执行完毕。`next`返回的value，是Generator向外输出的数据；`next`方法还可以接受参数，向Generator函数体内输入数据。
```
  // 使用Generator，执行一个真实的异步任务
  var fetch = require('node-fetch');
  function* gen() {
    var url = 'https://api.github.com/users/github';
    var result = yield fetch(url);
    console.log(result.bio)
  }

  var g = gen();
  var result = g.next();
  result.value.then(function(data){
    g.next(data)
  })
```

##### 8.2.2 基于Thunk函数的Generator执行器
执行过程：将同一个回调函数，反复传入next方法的value属性。内部的next函数就是Thunk的回调函数。next函数先将指针移到Generator函数的下一步（gen.next方法），根据result.done判断函数是否结束，没结束将next函数再次传入。已结束，直接退出。只要 Generator 函数还没执行到最后一步，next函数就调用自身，以此实现自动执行。
```
var fs = require('fs');

var readFile = function (fileName){
  return new Promise(function (resolve, reject){
    fs.readFile(fileName, function(error, data){
      if (error) return reject(error);
      resolve(data);
    });
  });
};

var gen = function* (){
  var f1 = yield readFile('/etc/fstab');
  var f2 = yield readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```

手动执行上面的Generator函数
```
var g = gen();

g.next().value.then(function(data){
  g.next(data).value.then(function(data){
    g.next(data);
  });
});
```

手动执行，转换为自动执行器
```
function run(gen){
  var g = gen();

  function next(data){
    var result = g.next(data);
    if (result.done) return result.value;
    result.value.then(function(data){
      next(data);
    });
  }

  next();
}

run(gen);
```

<br>
<br>
### 参考资料
- 《深入理解ES6》NICHOLAS C.ZAKAS
- [《ECMAScript6入门》 阮一峰](http://es6.ruanyifeng.com/)