# 类

对于初学者来说，在JavaScript中定义类可能有些令人迷惑。没有类的声明，这一点体现了JavaScript并没有将类设计为制造对象的工厂。事实上，JavaScript是一门原型语言。

>原型继承是一种面向对象的代码重用形式。 JavaScript是使用原型继承的仅有的（主流）面向对象语言之一。几乎所有其他面向对象的语言都是经典继承的。
>
>在经典继承中，程序员编写一个类，该类定义一个对象。可以从同一类中实例化多个对象，因此我们只需要在一个地方写我们的代码。然后我们可以将类组织成层次结构，以促进代码重用。更通用的代码存储在较高级别的类中，较低级别的类从中继承。这意味着一个对象正在与同一类的其他对象及其父类共享代码。
>
>在原型继承形式中，对象直接从其他对象继承。关于类的所有业务都消失了。如果需要对象，则只需编写一个对象。但是代码重用仍然是很有价值的事情，因此允许将对象按层次结构链接在一起。在JavaScript中，每个对象都有指向创建它的对象的秘密链接，形成一个链。当要求对象提供其不具有的属性时，将询问其父对象，并一直沿链向上，直到找到该属性或到达根对象为止。 （http://stackoverflow.com/questions/186244/what-does-it-mean-that-javascript-is-a-prototype-based-language）

然而，这会给我们直接在JavaScript中使用经典的OOP带来困难。新版本的JavaScript版本带来了一些语法糖，可以使我们编写相关功能的时候更便利一点。

我们从一个定义分数（Fraction）的例子出发，它拥有如下功能：

- 给定分子和分母创建一个新的分数；
- 打印分数的时候应当是化简了的形式；
- 两个分数能加减；
- 两个分数能乘除；
- 两个分数之间能进行比较；
- 一个分数和一个整数能相加；
- 给出一个分数的列表，能使用默认的排序函数进行排序。

我们在Python中使用如下实现：

```python
class Fraction:
    def __init__(self, top, bottom):
        self.num = int(top) # the numerator is on top
        self.den = int(bottom) # the denominator is on the bottom

    def __repr__(self):
        if self.num > self.den:
            retWhole = int(self.num / self.den)
            retNum = self.num - (retWhole * self.den)
            return str(retWhole) + " " + str(retNum) + "/" + str(self.den)
        else:
            return str(self.num) + "/" + str(self.den)

    def show(self):
        print(str(self.num) + "/" + str(self.den))

    def __add__(self, otherfraction):
        # convert to a fraction
        otherfraction = self.toFrac(otherfraction)

        newnum = self.num * otherfraction.den + self.den * otherfraction.num
        newden = self.den * otherfraction.den

        common = gcd(newnum, newden)

        return Fraction(newnum / common, newden / common)

    def __radd__(self, leftNum):
        otherfraction = self.toFrac(leftNum)            
        newnum = self.num * otherfraction.den + self.den * otherfraction.num
        newden = self.den * otherfraction.den

        common = gcd(newnum, newden)

        return Fraction(newnum / common, newden / common)

    def __cmp__(self, otherfraction):
        num1 = self.num * otherfraction.den 
        num2 = self.den * otherfraction.num

        if num1 < num2:
           return -1
        else:
           if num1 == num2:
              return 0
           else:
              return 1

    def toFrac(self, n):
        if isinstance(n, int):
            otherfraction = Fraction(n, 1)
        elif isinstance(n, float):
            wholePart = int(n)
            fracPart = n - wholePart
            fracNum = int(fracPart * 100)
            newNum = wholePart * 100 + fracNum
            otherfraction = Fraction(newNum, 100)
        elif isinstance(n, Fraction):
            otherfraction = n
        else:
            print("Error: cannot add a fraction to a ", type(n))
            return None
        return otherfraction

#gcd is a helper function for Fraction
def gcd(m,n):
    while m % n != 0:
        oldm = m
        oldn = n

        m = oldn
        n = oldm % oldn

    return n
```

分数类所需的实例变量（数据成员）是分子和分母（注意我在这里做了一个int型转换）。 当然，在Python中，我们可以随时通过向`objectReference.variableName`赋值来将实例变量添加到类中。

在JavaScript中：

```javascript
"use strict"
class Fraction {
    constructor(num, den) {
        this._numerator = num;
        this._denominator = den;
    }

    toString() {
        return `${this.numerator} / ${this.denominator}`;
    }

    get numerator() {
        return this._numerator;
    }

    get denominator() {
        return this._denominator;
    }

    set numerator(val) {
        console.log("error cannot set the numerator");
    }
}

let x = new Fraction(2, 3);
console.log("x is " + x)
console.log(x.numerator)
```

一个旧式的写法可能是：

```javascript
function Fraction(num, den) {
    this.numerator = num;
    this.denominator = den;
}

Fraction.prototype.toString = function() {
    return this.numerator + " / " + this.denominator;
}

let f = new Fraction(2, 3);
console.log("f is " + f)
```



## 方法和成员函数

现在我们来看看JavaScript和Python之间的主要区别之一。 Python类定义使用特殊的方法进行加法和比较，从而重新定义了标准运算符的行为。 在JavaScript中，没有运算符重载的概念。 因此，我们将不得不编写成员函数来执行加、乘和除运算。 让我们从加法开始：

```javascript
"use strict";
class Fraction {
    constructor(num, den) {
        this._num = num;
        this._den = den;
    }

    toString() {
        return `${this.numerator} / ${this.denominator}`;
    }

    get numerator() {
        return this._num;
    }

    get denominator() {
        return this._den;
    }

    set numerator(val) {
        console.log("error cannot set the numerator");
    }

    gcd(m, n) {
        while (m % n != 0) {
            let oldm = m;
            let oldn = n;
            m = oldn;
            n = oldm % oldn;
        }
        return n;
    }

    add(other) {
        let newDen = this.denominator * other.denominator;
        let newNum = this.numerator * other.denominator + other.numerator * this.denominator;
        let common = this.gcd(this.denominator, other.denominator);
        return new Fraction(newNum / common, newDen / common);
    }

}

let x = new Fraction(2, 3);
let y = new Fraction(1, 6);
console.log("x is " + x);
console.log(x.numerator);
console.log("x + y = " + x.add(y));
```

向类添加方法的语法非常简洁，但是在方法名称前没有单词`function`或`def`可能会有些奇怪。 在所有方法中，`this`均指当前对象，就像在Python中的`self`一样。可以注意到，我们不必将其作为每个方法的第一个参数提供。

## 继承

JavaScript中也支持继承。如下是一个Python中常见的例子：

```python
class Animal:
    def __init__(self, name):
        self._name = name

    def speak(self):
        print("Generic Animals are mute")

    def get_name(self):
        return "My name is {}".format(self._name)

    name = property(get_name)


class Dog(Animal):
    def __init__(self, name):
        super().__init__(name)
        self.numLegs = 4

    def speak(self):
        print("woof woof")


class CartoonDog(Dog):
    def speak(self):
        print("I'll have a dry martini.")

spot = Dog("spot")
print(spot.name) # My name is spot
spot.speak() # woof woof

brian = CartoonDog("Brian")
brian.speak() # I'll have a dry martini.
print(brian.name) # My name is Brian
```

在JavaScript中，使用关键字`extends`来实现继承。如下面的示例所示：

```javascript
"use strict";
class Animal {
    constructor(name) {
        this._name = name;
    }

    speak() {
        console.log("Generic animals are Mute");
    }

    get name() {
        return this._name;
    }
}

class Dog extends Animal {
    constructor(name) {
        super(name);
        this.numLegs = 4;
    }

    speak() {
        console.log("woof woof");
    }
}

class CartoonDog extends Dog {
    speak() {
        console.log("I'll have a dry martini.")
    }

}

let spot = new Dog('spot');
spot.speak(); // woof woof
console.log(spot.numLegs); // 4
let brian = new CartoonDog('Brian');
brian.speak(); // I'll have a dry martini.
console.log(brian.name); // Brian
```

