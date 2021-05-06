# Web编程

JavaScript在现代浏览器中最核心的功能之一就是操作web页面中的元素，目前事实上的标准就是采用文档对象模型（[DOM](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model/Introduction)）并对它进行各类操作：添加、更新、删除等。比如说，你可以选择按时间间隔来不断更新一个元素来达到显示动画的效果。DOM并不是一个编程语言，最开始的时候，JavaScript和DOM是交织在一起的，但它们最终演变成了两个独立的实体。JavaScript和Python都可以访问和操作存储在DOM中的内容，但本文的这个部分就不对Python的功能做过多的阐述——毕竟在web编程部分，JavaScript是一门更好的语言；同时，之前的部分也已经对两种语言做了一些方面的比较，相信到这里你也对JavaScript有了基本的了解。本章分为如下几个部分：首先，我会简单描述一下使用原生的JavaScript来操作DOM；然后，我对两个流行的JavaScript库，jQuery和Underscore，进行简单的介绍；最后，我会介绍node.js——当前最流行的跨平台JavaScript执行环境之一。

## DOM
DOM是一个树形的结构，基本操作包括：更新、添加、删除和遍历。要对DOM节点进行操作，我们首先要获得这个节点的引用：最常用的方法有`getElementById()`、`getElementsByTagName`和`getElementsByClassName`。我们可以从语法上发现一个细节：使用id来获取的元素是唯一的，而另外两种方法则会拿到一组节点。同时，我们也可以使用`querySelector()`和`querySelectorAll()`来选择元素。相关的语法可以查阅：https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelector。

在我们拿到想要操作的DOM元素之后，它作为一个对象，我们可以直接对其属性进行修改：
- 修改`innerHTML`属性；
- 修改`innerText`；
- 修改`innerContent`。

由于整个html页面被解析成一个DOM树，要插入节点的话，我们可以先拿到想插入的位置的父节点，然后使用`parent.appendChild()`。另一种方法是使用`parent.insertBefore(newElement, refElement);`插入到`refElement`的前面。想要删除节点的话，直接调用父节点的`removeChild()`即可。

有时候，我们会使用`parent.children`来遍历`parent`节点的子节点。这个时候需要注意的是，`children`实际上是一个引用，在任何时刻我们对`parent`的子节点进行了更改，`children`的内容都会改变。举一个例子：
```html
<div id="test">
    <ol id="test-list">
        <li class="lang">Scheme</li>
        <li class="lang">JavaScript</li>
        <li class="lang">Python</li>
        <li class="lang">Ruby</li>
        <li class="lang">Haskell</li>
    </ol>
</div>
```
我们使用下面的代码来删除`test-list`中元素：
```javascript
let elem = document.getElementById('test-list');
for (let i of elem.children) {
    elem.removeChild(i);
}
```
如例子中所示，`test-list`中有5个元素，那么上述代码能删除多少个呢？答案是3个！我们很容易发现，被删除的元素是第1、3、5个元素。原因很简单，在遍历的过程中，`elem.children`也在动态改变。在删除了`Scheme`之后，列表中只剩下四个元素，这个时候，`i`指向第二个元素，也就是`Python`，这个时候进行删除操作，就会删掉它，也就是原来的第三个元素。

## jQuery

### 操作DOM
[jQuery](https://jquery.com/)封装了常用的JavsScript操作，并提供更简洁的语法。在使用的时候，我们可以直接调用`jQuery.someMethod`或者使用它的别名`$`。一个简单的查找DOM的操作为：
```javascript
let elem = $('#test-form'); // 查找id为test-form的DOM节点
```
注意，`$()`返回的是一个类似数组的对象，即使只查找到一个元素，那也会返回形如`[someElem]`的对象。常用的查找操作有：
```javascript
$('div') // 查找所有<div>节点
$('.red') // 查找所有class包含red的节点
$('[attr=val]') // 查找所有attr为val的节点
$('[attr^=pre]') // 查找前缀为pre的节点
$('[attr$=suf]') // 查找后缀为suf的节点
$('div.green, p.red') // 多项选择，用','隔开
$('div p.green') // 层次选择，用' '隔开
```
选择出来的jQuery对象也拥有`filter`和`map`这样的方法，只需要传入对应的函数可以实现和Array一样的效果。

在拿到了jQuery对象之后，对DOM节点进行操作的常用方法有：
```javascript
$('cond').text(str) // 若str为空，则返回其文本
$('cond').html(htmlStr) // 若htmlStr为空，则返回其HTML
$('cond').css(attrName, val) // 修改css，支持链式调用
$('cond').show() // 显示DOM
$('cond').hide() // 隐藏DOM
$('cond').attr(attrName, val) // 修改对应的属性
$('cond').prop(attrName, val) // 同上，两者的返回值略有不同，使用prop更好

$('cond').append(obj) // 给DOM对象添加新的节点或函数对象
$('cond').remove() // 删除DOM节点
```

### 响应事件
JavaSript是单线程运行的语言，依赖触发事件来执行对应的代码。使用jQuery，我们可以不用操心不同浏览器之间的差异，直接使用同一套代码：
```javascript
$('#test-form').on('submit', function () {
    alert('Submit!');
}) // id为test-form的节点响应submit事件

// 更简洁的写法是
$('#test-form').submit(function(e) {}); // 其中，e是事件参数 

// 更多例子
$('cond').click(function(e) {}); // 鼠标单击时触发
$('cond').change(function(e) {}); // 当相关内容改变时触发
$('cond').focus(function(e) {}); // DOM获得焦点时触发
$('cond').ready(function(e) {}); // DOM初始化完成后出发
```

### AJAX
使用jQuery来处理AJAX非常方便：
```javascript
$.ajax(url [, settings]) // url 为发送request的地址，settings为其对应的配置项，它是一个object
```
我们可以链式调用一系列方法，来处理对应的请求：
```javascript
$.ajax({
  url: "test.html",
  context: document.body
}).done(function() {
  $( this ).addClass( "done" );
});
```


以上只是一些简单的示例，jQuery拥有非常强大的功能，更多的细节建议查阅：https://learn.jquery.com/

## Underscore
[Underscore](https://underscorejs.org/)是另外一个流行的第三方开源库，它提供了一套完整的函数式编程接口，可以作用于集合类对象之上。集合类指Array和Object，一些简单的例子是：
```javascript
_.each([1, 2, 3], alert); // alerts each number in turn...
_.each({one: 1, two: 2, three: 3}, alert); // alerts each number value in turn...

_.map([1, 2, 3], function(num){ return num * 3; }); // [3, 6, 9]
_.map({one: 1, two: 2, three: 3}, function(num, key){ return num * 3; }); // [3, 6, 9]
_.map([[1, 2], [3, 4]], _.first); // [1, 3]

var sum = _.reduce([1, 2, 3], function(memo, num){ return memo + num; }, 0); // 6 

var even = _.find([1, 2, 3, 4, 5, 6], function(num){ return num % 2 == 0; }); // 2

_.where(listOfPlays, {author: "Shakespeare", year: 1611}); // [{title: "Cymbeline", author: "Shakespeare", year: 1611}, {title: "The Tempest", author: "Shakespeare", year: 1611}]

var evens = _.filter([1, 2, 3, 4, 5, 6], function(num){ return num % 2 == 0; }); // [2, 4, 6]

_.every([2, 4, 5], function(num) { return num % 2 == 0; }); // false

_.groupBy([1.3, 2.1, 2.4], function(num){ return Math.floor(num); }); // {1: [1.3], 2: [2.1, 2.4]}
```
更多的使用方法请查阅：https://underscorejs.org/

## Node.js
[Node.js](https://nodejs.org/)是一个基于Chrome V8引擎的JavaScript运行时。如果你熟悉各类Python的解释器，那么理解它也会非常容易。它能提供服务端的JavaScript支持，非常强大，一个简单的例子如下：
```javascript
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

它对事件模型有着更深入的设计，使用一个事件轮询作为运行时构造而不是库。
> 通常 Node.js 的行为是通过在脚本开头的回调定义的，在结束时通过阻塞调用（如 EventMachine::run() ）启动服务器。在 Node.js 中没有这样的启动-事件循环调用。Node.js 在执行输入脚本后只需输入事件循环即可。 当没有更多要执行的回调时，Node.js 退出事件循环。此行为类似于浏览器中的 JavaScript ——事件循环总是对用户不可见的。

同时，Node.js的设计使它能更方便地设计为提供服务端的功能。
> HTTP 是 Node.js 中的一等公民。它设计的是流式和低延迟。这使得 Node.js 非常适合于 web 库或框架的基础。仅仅因为 Node.js 是在没有线程的情况下设计的，这并不意味着您无法利用环境中的多个内核。子进程可以通过使用我们的 child_process.fork() API 来生成，并且被设计为易于沟通。建立在同一接口上的是 cluster 模块，它允许您在进程之间共享套接字，以便在核心上启用负载平衡。

了解更多相关的功能，请查阅：https://nodejs.org/
