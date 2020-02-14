# 函数

Python中可以定义普通函数和匿名函数：

```python
# 一个命名函数的例子：
def foo(x, *args, **kwargs):
    print(x)
    for arg in args:
        print(arg)
    for k, v in kwargs.items():
        print(k, ":", v)
    return x ** 2


# or using the lambda expression
foo2 = lambda x, y: x - y if x >= y else y - x
foo2(3, 4) == foo2(4, 3) # True

# or using IIFE (Immediately Invokable Function Expressions) if the function is only used once
(lambda x: x**2)(4) # 16
```

使用匿名函数会带来一些好处，比如不会污染我们的命名空间。注意到我将`lambda`定义的匿名函数赋给了`foo2`，这表示Python将函数视作头等公民，这一点我们稍后进行讨论。同时，如果我们只使用一次函数，那我们可以使用IIFE，简单来说就是直接调用此函数，这个特性在Python里使用的并不多，而在JavaScript中非常有用。这里需要注意的是，Python对于匿名函数的支持并不完全——`lambda`表达式只能写一行，这也显示出它并不支持`for`或者`while`。而JavaScript中对于匿名函数的支持是完全的，我们可以像定义一个命名函数那样定义匿名函数：

```javascript
function foo() {}
// or using the ECMAScript 2015 arrow notation
const foo = () => {};

function () {};
// or using the ECMAScript 2015 arrow notation
() => {};

// or IIFE
(function () {})()
(() => {})()
```

同时，在JavaScript和Python中，函数都是头等公民。这意味着，函数可以被当作参数传递给其他函数，可以作为另一个函数的返回值，还可以被赋值给一个变量。我们称其为头等函数。赋值给变量的例子上面已经展示过了，接下来我们一起看一下其他Python和JavaScript都有的特性：闭包、高阶函数以及生成器，这些都是将函数作为头等公民带来的好处。

## 闭包

函数与对其状态即**词法环境**（**lexical environment**）的引用共同构成**闭包**（**closure**）。也就是说，闭包可以让我们从内部函数访问外部函数作用域。让我们看一个闭包的例子：

```python
def makeFunc():
    name = "my function"
    def showName():
        print(name)
    return showName

func = makeFunc()
func() # my function
```

在JavaScript中：

```javascript
function makeFunc() {
    let name = "my function";
    function showName() {
        alert(name);
    }
    return showName;
}

let myFunc = makeFunc();
myFunc(); // my function
```

在上述的例子中，`showName`函数到执行的时候仍然可以访问`name`变量，这是因为这里形成了一个闭包。闭包很有用，因为它允许将函数与其所操作的某些数据（环境）关联起来。这显然类似于面向对象编程。在面向对象编程中，对象允许我们将某些数据（对象的属性）与一个或者多个方法相关联。因此，通常你使用只有一个方法的对象的地方，都可以使用闭包。在 Web 中，我们想要这样做的情况特别常见。大部分我们所写的 JavaScript 代码都是基于事件的——定义某种行为，然后将其添加到用户触发的事件之上（比如点击或者按键）。我们的代码通常作为回调：为响应事件而执行的函数。这些内容我们会在Web编程中进行讨论。

## 高阶函数

当一个函数接收另外一个函数作为实参，我们管它叫高阶函数。假设你熟悉Map-Reduce模式，在Python和JavaScript中它们非常类似。不过，在Python中，几乎所有使用`map`的地方都可以使用列表推导式来进行实现，而后者可读性更强。不过，因为JavaScript对匿名函数有更好的支持，所以在JavaScript中`map`的使用非常广泛。我们直接看一下相关的例子：

```python
from functools import reduce # in Python 3

arr = [1, 2, 3]
pow_results = map(lambda x: x ** 2, arr) # [x ** 2 for x in arr]
sum_results = reduce(lambda x, y: x + y, arr)
even_results = filter(lambda x: x % 2 == 0, arr)
```

注意，在Python 3中，你需要从`functools`中导入`reduce`。上述的`*_results`都是生成器，我们接下来会提到如何使用。JavaScript中：

```javascript
let arr = [1, 2, 3];
let pow_results = arr.map((x) => x ** 2)
let sum_results = arr.reduce((x, y) => x + y)
let even_results = arr.filter((x) => x % 2 === 0)
```

可以看到，上述例子中，我们都是给一个函数传入另一个函数作为实参。同时，我们可以使用匿名函数作为参数，这样不会污染我们的函数命名空间。

## 生成器

在Python中，生成器（generator）的使用非常广泛。它可以节省大量内存，只在需要的时候返回值即可。一个生成Fibonacci数列的例子是：

```python
def genFib(num):
    n, a, b = 0, 0, 1
    while n < num:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'

list(genFib(6)) # [1, 1, 2, 3, 5, 8]
```

Python中的生成器也是迭代器，我们可以使用`next`来获取每次的返回结果，也可以使用`for`循环。上述例子中我使用了`list`来获取所有的结果。上述的`*_results`也都可以使用这个方法获取结果。在JavaScript中的生成器和Python非常类似，不过它使用`function*`语法：

```javascript
function* genFib(num) {
    let [n, a, b] = [0, 0, 1];
    while (n < num) {
        yield b;
        [a, b] = [b, a + b]
        n = n + 1
    }
    return "done";
}

let g = genFib(6);
g.next(); // {value: 1, done: false}
g.next(); // {value: 1, done: false}
g.next(); // {value: 2, done: false}

for (var x of g) {
    console.log(x); // 3, 5, 8
}
```

生成器给现代Web编程带来了巨大的便利，我们会在之后的章节中讲到。