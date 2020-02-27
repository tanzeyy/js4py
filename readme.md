# From Python to JavaScript

## 介绍

本文假设你已经对Python有一定的了解。首先，我们会从一个简单的JavaScript程序开始，这会让我们对这门语言有一个初步的印象。然后，我们会研究大多数编程语言都具有的主要结构：数据类型，循环语句和条件语句。当我们掌握了JavaScript的基础知识后，我们将继续研究JavaScript中独特又强大的功能：JavaScript的内置类和前端Web编程。

## 一个简单的程序

让我们从Hello World开始。在Python中，通常我们有两种方法来在屏幕上输出Hello World，一种是在Python解释器中直接打：

```python
print("Hello World!")
```

或者将上述代码保存为`.py`文件，并使用`python *.py`来运行。和Python一样，JavaScript是一门解释型脚本语言，但是它通常运行在现代浏览器中。虽然你可以使用如`node.js`来在命令行里面运行JavaScript代码，但是本文推荐你打开浏览器（Chrome或Firefox），按下F12，然后你就能看见一个类似IDLE的控制台。输入

```javascript
console.log("Hello World!");
```

敲回车之后，你可以看见控制台的输出。或者使用

```javascript
alert("Hello World!");
```

来弹出一个对话框。

之后，我们展示出的所有代码都可以在控制台里面运行。在本文中，我们会对比Python和JavaScript的一些基础知识。本文的结构如下：

- [基础](./doc/ch1_basics.md)
- [函数](./doc/ch2_functions.md)
- [类](./doc/ch3_classes.md)
- [Web编程](./doc/ch4_web_programming.md)

## 参考

JS4Python(https://runestone.academy/runestone/books/published/JS4Python/index.html)

MDN web docs(https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference)