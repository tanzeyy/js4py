# 基础

- 数据类型
- 循环语句
- 读取用户输入
- 条件语句

## 变量声明、作用域

Python和JavaScript都是动态类型语言，这意味着，一个变量可以在任何时间引用任何类型的对象。仅当使用这个变量的时候，解释器才会确定它是何种类型的对象。通常，我们在Python中声明一个变量时需要对它进行实例化：

```python
x = 1.6
d = dict()
```

这意味着，在Python中每一个可使用的变量都有一个其指向的对象。而在JavaScript里，我们可以先声明变量而不对它进行赋值，也可以在声明的同时赋值。声明变量可以使用`var`或者`let`：

```javascript
var a;
var a = 12.4;
let x;
let x = a * 10;
```

或者使用`const`来声明常量：

```javascript
const ratio = a / x;
```

注意，在JavaScript中变量只能声明一次，重复声明会报错。

乍看之下，使用`var`和`let`好像没有差别，实际上，它们的差别体现在变量的作用域上。先说结论：建议在代码中只使用`let`和`const`来声明变量。

通常我们讨论的变量作用域有三个范围：全局（global），函数（function）和块（block）。在Python中拥有特殊的`global`关键字，但由于Python特殊的GIL，我个人不推荐使用`global`，如果一定需要在模块之间共享变量请采用特殊方法（比如flask中的`g`）。这里我们不对Python的变量作用域做过多的讨论，让我们把注意力集中到JavaScript。在JavaScript中，`let`允许你声明一个作用域被限制在块级中的变量、语句或表达式，而`var`声明的变量只能是全局或整个函数块的，举个例子：

```javascript
function varTest() {
  var x = 1;
  {
    var x = 2;  // 同样的变量!
    console.log(x);  // 2
  }
  console.log(x);  // 2
}

function letTest() {
  let x = 1;
  {
    let x = 2;  // 不同的变量
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
```

在程序和方法的最顶端，`let`不像 `var` 一样，`let`不会在全局对象里新建一个属性。比如：

位于函数或代码顶部的`var`声明会给全局对象新增属性, 而`let`不会。例如:

```javascript
var x = 'global';
let y = 'global';
console.log(this.x); // "global"
console.log(this.y); // undefined
```

上述问题会导致什么呢？让我们看一个`for`循环的例子：

```javascript
'use strict';

function test() {
    for (var i = 1; i <= 10; i++) {
        console.log(i);
    }
    // 而此时i仍然可以被引用！
    console.log("此时的i为：", i);
}

test(); // 1 2 3 4 5 6 7 8 9 10 11
```

为了避免上述问题，JavaScript在早些年引入了`'strict mode'`。时至今日，最好的建议是在代码中只使用`let`和`const`来声明变量。



## 布尔运算符

布尔值只有`true`和`false`两种，可以直接用它们表示，也可以通过布尔运算计算出来。Python和JavaScript中常见的布尔运算符号有：

| 运算 | Python | JavaScript |
| :--: | :----: | :--------: |
|  与  |  and   |     &&     |
|  或  |   or   |    \|\|    |
|  非  |  not   |     !      |

也可以通过比较运算符`>`、`<`、`==`、`>=`和`<=`来得到一个布尔值，这些运算符在Python和JavaScript中都是相同的。然而，值得注意的是，JavaScript在实现`==`比较时，会自动进行数据类型转换，而`===`则会在数据类型不一致时返回`false`：

```javascript
false == 0; // true
false === 0; // false
```

因此，在JavaScript中推荐只使用`===`进行比较。

另一个不同点就是布尔表达式。在Python里面，你可能会使用如下的布尔表达式：

```python
x = value if condition else other_value
```

这是Python3的特性，注意通常来说这种写法会使代码的可读性下降。同样，JavaScript也支持布尔表达式：

```javascript
x = condition ? value : other_value
```

和Python不一样的是，这种把条件放在前面的表达式读起来比较迷惑，要注意区分。

当然，以上这些都可以用`if ... else`语句来完成：

```javascript
if (condition)
    x = value
else
    x = other_value
```

不过谁能拒绝只写一行代码的快乐呢？



## 条件语句

在Python中的条件语句和JavaScript中的非常相似。

### 1、if

Python中：

```python
if condition:
    statement1
    statement2
    ...
```

在JavaScript中：

```javascript
if (condition) {
    statement1
    statement2
    ...
}
```

再次可以看到在JavaScript中，我们使用花括号而不是缩进来定义一个块。 在JavaScript中，必须在条件周围加上括号，因为从技术上讲，`if`是一个评估为`True`或`False`的函数。

### 2、if else

```python
if condition:
    statement1
    statement2
else:
    statement1
    statement2
    ...
```

在JavaScript中写作：

```javascript
if (condition) {
    statement1
    statement2
    ...
} else {
    statement1
    statement2
    ...
}
```

### 3、elif

JavaScript并不像Python一样有`elif`，我们可以通过嵌套`if`和`else`来获得`elif`语句的功能。下面是一个简单的示例：

```python
grade = int(input('enter a grade'))
if grade < 60:
    print('F')
elif grade < 70:
    print('D')
elif grade < 80:
    print('C')
elif grade < 90:
    print('B')
else:
    print('A')
```

在JavaScript中可以写作：

```javascript
function main(grade) {
    if (grade < 60) {
        console.log('F');
    } else {
        if (grade < 70) {
            console.log('D');
        } else {
            if (grade < 80) {
                console.log('C');
            } else {
                if (grade < 90) {
                    console.log('B');
                } else {
                    console.log('A');
                }
            }
        }
    }
}
```

但这种写法看起来非常冗余。在JavaScript中，单行语句不一定需要用括号包起来。所以我们可以采用如下的写法：

```javascript
function main(grade) {
     if (grade < 60) {
         console.log('F');
     } else if (grade < 70) {
         console.log('D');
     } else if (grade < 80) {
         console.log('C');
     } else if (grade < 90) {
         console.log('B');
     } else console.log('A');
}
```

### 4、switch

JavaScript还支持`switch`语句，该语句在某些情况下的行为类似于Python的`elif`语句。我们可以使用`switch`语句编写上述程序：

```javascript
"use strict";
function main(grade) {
   let tempgrade = Math.trunc(grade / 10);
   switch(tempgrade) {
   case 10:
   case 9:
       console.log('A');
       break;
   case 8:
       console.log('B');
       break;
   case 7:
       console.log('C');
       break;
   case 6:
       console.log('D');
       break;
   default:
       console.log('F');
   }
}
```

**注意**，`switch`语句的条件能力较弱，且容易忘记放`break`，所以实际情况中不推荐使用`switch`语句。



## 循环和迭代

循环和迭代是基本操作，这里也给出一些Python和JavaScript中的相关比较。

### 1、有限循环

Python中最简单的例子就是使用`for`和`range`：

```python
for i in range(10):
    print(i)
```

在JavaScript中则是：

```javascript
for (let i = 0; i < 10; i++) {
    console.log(i);
}
```

回顾一下`range`的用法：

```python
range(stop)
range(start, stop)
range(start, stop, step)
```

其中，`start`、`stop`和`step`分别表示开始、结束和步长。在JavaScript中，写作：

```javascript
for (start clause; stop clause; step clause) {
    statement1
    statement2
    ...
}
```

举个例子，从100开始，到0结束，步长为5的Python循环可以写作：

```python
for i in range(100, -1, -5):
    print(i)
```

在JavaScript中则为：

```javascript
for (let i = 100; i >= 0; i -= 5) {
    console.log(i);
}
```

得益于**鸭子类型**，在Python中的`for`循环可以用来迭代*任何*序列，只要这个类实现了相关的接口（i.e., `__iter__`方法）。JavaScript也提供了类似的循环。

在Python中，迭代一个list可以使用：

```python
l = [1, 2, 3, 4, 5]
for i in l:
    print(i)
```

而在JavaScript中也可以这样迭代一个*可迭代对象*，比如说array：

```javascript
let l = [1, 2, 3, 4, 5]
for (let i of l) {
    console.log(i);
}
```

实际上，`for ... of`是ES6新提供的特性（[参考阅读](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)），更多例子里也使用`for ... in`：

```javascript
let l = [1, 2, 3, 4, 5]
for (let i in l) {
    console.log(l[i]);
}
```

很容易发现，`for .. of`遍历的是value，而`for ... in`是key。换句话说，前者遍历的是对象元素，后者遍历对象属性。注意，`for ... of`无法用来循环普通对象，需要搭配`Object.keys()`使用。

像这样的循环我们通常管它叫`for each`循环，实际上，ES5就引入了内建的`forEach`，只是它实在不太好用。到今天，大多数现代编程语言都提供了类似的功能，此处不再赘述。

### 2、无限循环

大多数语言的`while`循环都很相似，在Python中写作：

```python
while condition:
   statement1
   statement2
   ...
```

在JavaScript中则只需要加上括号：

```javascript
while (condition) {
   statement1
   statement2
   ...
}
```

JavaScript还提供了`do ... while`循环，类似于C，它的循环条件是写在语句块的末尾的：

```javascript
do {
    statement1
    statement2
    ...
} while (condition);
```



## 基本数据类型

我们都知道，程序是由数据结构和算法组成的。JavaScript中有如下几种基本数据类型：

- boolean

- number
- string
- undefined
- null
- symbol (new)

### 1、boolean

前面已经提到过有关布尔值的评估问题。这里提一下特例。

在Python中，空的list、空的string和空的dict都会被评估为`False`。而在JavaScript里面，只有：`null`、`undefined`、`NaN`、`0`、`""`和`false`会被评估为`false`（注意`f`是小写），其他的所有都是`true`。

### 2、number

Python和JavaScript在数值处理方面都比较灵活，值得注意和加以区分的是NaN与Inf。在Python中，它们可以表示为：

```python
# 使用float
nan = float('nan') # type is float
inf = float('inf')

# 使用decimal
from decimal import Decimal
nan = Decimal('nan') # type is decimal.Decimal
inf = Decimal('inf')

# 使用numpy
import numpy as np
nan = np.nan # type is float
inf = np.inf
```

上述的`nan`和`inf`的类型略有不同，但在Python中变量类型的处理比较灵活，此处也并没有大碍。在判断对应的变量时可以统一采用：

```python
import math
math.isnan(nan) # True
math.isinf(inf) # True
```

在JavaScript中，`NaN`和`Inf`有内置的number类型对应的实现，它们都是合法的number类型：

```javascript
let nan = NaN
let inf = Infinity
```

值得注意的是，`NaN`和任何值都不相等，包括它自己。唯一能判断的方法是通过`Number.isNaN()`函数：

```javascript
NaN === NaN; // false
isNaN(NaN); // true
```

而`Infinity`的表现则符合通常逻辑：

```javascript
const maxNumber = Math.pow(10, 1000); // max positive number

if (maxNumber === Infinity) {
  console.log("Let's call it Infinity!");
  // expected output: "Let's call it Infinity!"
}

console.log(1 / maxNumber); // expected output: 0

// More cases
console.log(Infinity          ); /* Infinity */  
console.log(Infinity + 1      ); /* Infinity */   
console.log(Math.log(0)       ); /* -Infinity */
console.log(1 / Infinity      ); /* 0 */
```

当然，也可以通过`Number.isFinite()`来判断：

```javascript
isFinite(5); // true
isFinite(maxNumber); // false
isFinite(Infinity); // false
```

### 3、string

#### 字符串基础

和Python一样，JavaScript的string也是不可变的。Python可以对字符串直接使用运算符进行索引和切片操作，而在JavaScript里面则是通过调用相关的方法来进行。下面是一个对比的表格：

| Python             | JavaScript           | 描述                                   |
| ------------------ | -------------------- | -------------------------------------- |
| `str[3]`           | `str.charAt(3)`      | 返回第三个位置的字符                   |
| `str[2:5]`         | `str.substring(2,4)` | 返回第二个位置到第四个位置的子串       |
| `len(str)`         | `str.length`         | 返回字符串的长度                       |
| `str.find('x')`    | `str.indexOf('x')`   | 在str中查找x出现的第一个位置           |
| `str.split()`      | `str.split(/\s+/)`   | 按空格将字符串str拆分成字符串列表/数组 |
| `str.split(',')`   | `str.split(',')`     | 按','字符的位置将str拆分成列表/数组    |
| `str1 + str2`      | `str1.concat(str2)`  | 将两个字符串str1和str2连接在一起       |
| `str.strip()`      | `str.trim()`         | 移除str开头或者结尾的空格              |
| `str.replace(a,b)` | `str.replace(a,b)`   | 将str中所有的a替换为b                  |

注意，在Python中，我们可能习惯使用`in`关键字来判断某个字符串是否是另一个字符串的子串，在JavaScript里我们使用`indexOf`。它会在查找失败的时候返回-1，和Python里的`find`的行为一致。下面举一个简单的例子：

```python
def removeVowels(s):
    vowels = "aeiouAEIOU"
    sWithoutVowels = ""
    for eachChar in s:
        if eachChar not in vowels:
            sWithoutVowels = sWithoutVowels + eachChar
    return sWithoutVowels

print(removeVowels("compsci")) # cmpsc
print(removeVowels("aAbEefIijOopUus")) # bfjps
```

上述程序用来移除某个字符串里的所有元音，而在JavaScript里的实现则可以为：

```javascript
function removeVowels(s) {
    const vowels = "aeiouAEIOU";
    let sWithoutVowels = "";
    for (let eachChar of s) {
        if (vowels.indexOf(eachChar) === -1) {
            sWithoutVowels = sWithoutVowels + eachChar
        }
    }
    return sWithoutVowels
}
```

这里我们再次回顾了`for`循环，可以看到，`for (let eachChar of s)`和Python中的`for eachChar in s`是最佳替代项。

#### 多行字符串

Python中的多行字符串可以使用三重引用号：

```python
s = """This is a 
multiline
string.
"""
```

在ES6标准之前，JavaScript中并没有这么便利的功能，现在则可以使用反引号：

```javascript
let s = `This is a
multiline
string, too.
`
```

这种情况下，每一行的末尾会显式地加上换行符。

#### 格式化字符串

字符串的一个常见操作则是处理格式化字符串，Python中有很多种方法：

```python
total = 10

mystr1 = "The total is %d." % total
mystr2 = "The total is {}.".format(total)
mystr3 = f"the total is {total}." # Python 3.6引入
```

注意第三种方式，字符串前的`f`不要遗漏。

在JavaScript中，格式化字符串叫做模板字符串（Template literals），和上述第三种方式非常接近。同时，像多行字符串一样，它们也由反引号分隔：

```javascript
total = 10
let mystr = `The total is ${total}`
```

JavaScript中的模板字符串非常强大，它们甚至可以包括索引对象。这一块我们留到之后再进行详细讨论。

### 4、null & undefined

如果要表示缺少对象或值，在Python中，我们使用`None`。而在JavaScript中，则使用`null`。在JavaScript中，还有一种特殊的类型叫`undefined`，用来表示未分配值的变量。 如果一个函数没有显式返回一个值，那么它返回的值也是`undefined`。

### 5、symbol

这个属于高级话题。

## 集合

我们现在来讨论JavaScript中的列表（list）和字典（dictionary）。首先，和Python一样，JavaScript也是一门面向对象的语言。同时，在JavaScript里，任何对象都表现得像字典一样，你可以给任何对象加上一个属性：`obj.newattr = value`。

### 列表/数组（List/Array）

List是Python的基本数据类型之一。JavaScript里没有List，但与之对应的有Array。这种数据类型，我们可以称之为有序序列，主要支持如下基本操作：

- append, pop
- index
- slice
- in / not in
- Creating lists of iterables

| Python                 | JavaScript                | 说明                                    |
| ---------------------- | ------------------------- | --------------------------------------- |
| l.append(newitem)      | l.push(newitem)           | 在末尾加入元素                          |
| l.pop()                | l.pop()                   | 移除末尾的元素                          |
| l.pop(0)               | l.shift()                 | 从开头移除元素                          |
| l.insert(0, newitem)   | l.unshift(newitem)        | 在开头添加元素                          |
| l.insert(idx, newitem) | l.splice(idx, 0, newitem) | splice在idx位置删除0个元素，插入newitem |
| del l[idx]             | l.splice(idx, 1)          | 删除idx位置的元素                       |
| l1 + l2                | l1.concat(l2)             | 将l1和l2拼接在一起                      |

最后一个需要关注一下。JavaScript里的Array并没有实现`+`操作符，但直接使用的话也不会报错。它会自动将两者的类型转成string，然后进行拼接操作：

```javascript
[1, 2] + [3, 4] // --> "1,23,4"
```

Python和JavaScript都支持索引操作，但是JavaScript不能使用复数作为索引。

切片是Python列表的灵魂操作之一，在JavaScript中，对应的是`slice`：

| Python   | JavaScript      | 说明                   |
| -------- | --------------- | ---------------------- |
| l[2:4]   | l.slice(2, 4)   | 从2切片到3，左闭右开   |
| l[2:]    | l.slice(2)      | 从2切片到末尾          |
| l[:4]    | l.slice(0, 4)   | 从开头切片到3          |
| l[-5:-1] | l.slice(-5, -1) | 从末尾的5切片到末尾的1 |

与索引不同，`slice`方法能接受负数作为与列表末尾的偏移量，并作为起始值和结束值。

#### 检查元素是否存在

和string一样，我们也使用`in`关键字来判断Python里的数组是否包含某个元素。除了和string一样使用`indexOf`，在JavaScript里我们也可以用如下方法：

在Python中，我们经常写`if something in mylist:`，在JavaScript里面我们有多种写法：

```javascript
let mylist = [1, 2, 3, "foo", "bar"];
if (mylist.includes(3)) {
    console.log('yes, includes returns true');
}
// indexOf returns the location of item or -1 if not found
if (mylist.indexOf(3) > -1) {
    console.log('yes, index is > -1');
}

// BEWARE this does not work as expected
if (3 in mylist) {
    console.log('yes, 3 is in mylist');
}

if ("foo" in mylist) {
    console.log('yes, foo is in mylist');
} else {
    console.log("what?");
}
```

注意！请关注后两种方法，这里使用了`in`关键字。在Python里，这是非常常用的方法，乍看起来它在JavaScript里面也行得通。然而，在上面谈到string的时候我们已经提到字符串是不支持`in`的，为什么这里的数组又支持了呢？实际上，这里`in`的行为和Python中的并不一致，这里的例子有误！在JavaScript中，`in`操作符仅仅对对象的键有用，而一个数组中的元素并不是对象的键，但数组也是对象，所以上述语句并不报错。我们可以使用下面的代码来查看一个对象的所有键：

```javascript
let mylist = [1, 2, 3, "foo", "bar"];
for (let k of Object.keys(mylist)) {
    console.log(k);
}
// 0 1 2 3 4
```

我们可以看到，数组的键是其索引的值。

#### 创建array

最后，我们看一下一些创建array的方法。我会描述一些常见的创建数组的场景。

首先，让我们直接从对应的类来构造数组实例。在Python中有一些常见的写法：

```python
arr = list()
arr2 = []
arr3 = [0] * 10 # [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

前两种都是创建一个空数组，第三行为创建一个有10个0元素的数组。对应的JavaScript代码为：

```javascript
let arr = Array()
let arr2 = []
let arr3 = Array(10).fill(0) // [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

然后，让我们看一下Python中从字符串创建数组的场景：

```python
s = "the quick brown fox jumps over";
l1 = s.split()
l2 = list(s)

print(l1) # ['the', 'quick', 'brown', 'fox', 'jumps', 'over']
print(l2) # ['t', 'h', 'e', ' ', 'q', 'u', 'i', 'c', 'k', ' ', 'b', 'r', 'o', 'w', 'n', ' ', 'f', 'o', 'x', ' ', 'j', 'u', 'm', 'p', 's', ' ', 'o', 'v', 'e', 'r']
```

上述`l1`是将字符串按空格拆分，`l2`是按字符拆分。下面是JavaScript中的一种实现：

```javascript
let s = "the quick brown fox jumps over";
let l1 = s.split(" ")
let l2 = Array.from(s)

console.log(l1) // ['the', 'quick', 'brown', 'fox', 'jumps', 'over']
console.log(l2) // ['t', 'h', 'e', ' ', 'q', 'u', 'i', 'c', 'k', ' ', 'b', 'r', 'o', 'w', 'n', ' ', 'f', 'o', 'x', ' ', 'j', 'u', 'm', 'p', 's', ' ', 'o', 'v', 'e', 'r']
```

接下来，我们讨论字典/对象。

### 字典/对象（Dictionary/Object）

正如我们希望轻松访问键值对时使用Python提供的字典一样，JavaScript也提供了类似的机制。实际上，JavaScript中的所有对象都拥有类似的功能。

```javascript
let x = {};
x['foo'] = 'bar';
x[2] = 99;
x.answer = 42; // Provied by JavaScript

console.log(x); // {2: 99, foo: "bar", answer: 42}
```

Python字典和JavaScript之间的一个重要区别是，在JavaScript中你可以使用`.`操作符来添加和检索为键存储的值。 当然，如果键的中间有空格或破折号，则只能使用索引符号，但是对于许多键，使用`.`操作符非常方便。

Python中对字典提供了取所有键的操作`myDict.keys()`和所有值的操作`myDict.values()`。在JavaScript中相同的功能略有些复杂：

```javascript
// 取键
const myDict = {foo: "bar", baz: 22, 33: 'hello'};
const keys = Object.keys(myDict);
console.log(keys); // ["33", "foo", "baz"]

// 取值
let vals = []
for (const key of Object.keys(myDict)) {
    vals.push(myDict[key]);
}
console.log(vals); // ["hello", "bar", 22]
```

可以看到，在JavaScript中，所有对象的键都是字符串，这点与Python不同。在Python中，键可以是任何可哈希的对象。这与JavaScript是为浏览器构建的web编程语言有关。在新版本的JavaScript中，上述取值方法可以写的更简单一点，那就是使用`map`函数：

```javascript
const myDict = {foo: "bar", baz: 22, 33: 'hello'};
const vals = Object.keys(myDict).map(key => myDict[key]);
console.log(vals); // ["hello", "bar", 22]
```

这里`map`方法将一个函数应用到`keys`中的所有元素上。我们会在之后对相关的函数进行讨论，这里只是展示一个简单的例子。

在Python中，我们还会使用`myDict.items()`来遍历所有键值对。在JavaScript中并没有严格对应的方法，使用`keys()`来遍历即可。除此之外，获取键的值（如果存在），否则获取默认值也是常见的操作`myDict.get(key, "default")`。在JavaScript中，习惯的写法是：

```javascript
myDict['no_key'] || 'default'
```

如果`'no_key'`不存在，那么`myDict['no_key']`就会返回`undefined`，那么上述表达式的值就会变为`'default'`。

在Python中，如果我们要更新一个对象的多个属性，我们可以直接使用`update`：
```python
obj = {"x": 2, "y": 4}
obj.update({"x": 3, "z": 5})

print(obj) # {'x': 3, 'y': 4, 'z': 5}
```

而在JavaScript中，我们可以使用`Object.defineProperties`，它本质上定义了`obj`对象上`props`的可枚举属性相对应的所有属性。
```javascript
var obj = {};
Object.defineProperties(obj, {
  'property1': {
    value: true,
    writable: true
  },
  'property2': {
    value: 'Hello',
    writable: false
  }
  // etc. etc.
});
```