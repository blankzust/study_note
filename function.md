# 函数

## 函数定义

* 函数声明

  ```js
  myFunction()	// 不会报错
  function myFunction() { console.log("myFunction") } // 函数声明
  myFunction.toString() // 返回："function myFunction() { console.log("myFunction") }"
  ```

  函数声明定义的函数，内部记录了函数名，在toString的时候会打印出来

* 表达式方式

  ```js
  myFunction();	// 报错：myFunction is not a function
  var myFunction = function() { console.log("myFunction"); } // 表达式定义
  myFunction.toString() // 返回"function() { console.log("myFunction") }"
  
  // 也可这样定义，但是表达式方式来定义函数一般不需要名称，这会让定义它们的代码变得紧凑
  var myFunctionWithName = function myFunctionWithName() { 
      console.log("myFunctionWithName") 
  }
  ```

  表达式方式先声明了一个匿名函数，将其引用赋予变量myFunction

* 嵌套函数

  函数定义可出现在全局代码里，或者内嵌在其他函数中，当内嵌在其他函数中时，我们称为嵌套函数

  ```js
  // 计算a*a + b*b
  function hypoteuse(a, b) {
      function square(x) { 
          console.log(a, b);  // 嵌套函数内部的作用域规则：可以访问嵌套它们的函数的参数和变了
          return x*x 
      }
      return square(a) + square(b);
  }
  ```

* 函数声明语句并非真正的语句，ECMAScript规范只是允许它们作为顶级语句

  ```js
  if( 1 = 1) {
      // 报错，函数声明无法处于if、for、with等语句中:Uncaught ReferenceError: Invalid left-hand side in assignment
      function myFunc() {
      	
      }
  }
  ```

## 函数调用

### 作为函数调用

`即正常地调用已经定义的函数`

```js
// 判断当前环境是否处于严格模式
function isStrict() {
    return !this;
}
isStrict();
```

* 这种方式调用的函数内部一般不使用this，this的值（调用上下文）是全局对象。
* 严格模式下，this的值为undefined

### 方法调用

`方法是指保存在一个对象的属性里的JavaScript函数`

```js
o.m = f; // o.m就是一个方法
o.m()	 // 方法的调用
var arr = [];
```

* 方法调用和函数调用的区别

  * 调用上下文不同
    * 函数调用的上下文为全局对象
    * 方法调用的上下文就是对象o
    * 方法内的嵌套函数的上下文为全局对象

* 方法链

  ```js
  var arr = [];
  arr.sort().reverse()	// 这是一个典型的方法链
  var o = {
      data: 0,
      add: function(value) {
          this.data += value;
          return this;
      },
      // 形成一个方法链的基本要求是：方法需要返回自身
      del: function(value) {
          this.data -= value;
          return this
      }
  }
  o.add(1).del(2)		// 这种方法链的写法很简洁
  ```

### 构造函数调用

`函数或者方法在调用之前带有关键字new时，它就构成构造函数调用`

```js
function constructFunc() {
    this.x = 1;
}
constructFunc.prototype = {
    data: 0
}
var o = new constructFunc();
var oj = new constructFunc;		// 这种方式也是可以的,省略括号和括号中的参数
o.data;			// 返回为0，data为o的继承属性
o.x;			// 返回为1，x为o的自有属性
```

··

`构造函数通常不使用return关键字`

```js
// 返回对象的构造函数
function funcWithReturnObj() {
    return { x: 1 }
}

var obj = new funcWithReturnObj();
obj.x		// obj为构造函数返回的对象{ x:1 }

// 返回非对象的构造函数
function funcWithReturnNonObj() {
    this.x = 1;
    return 1
}

var obj = new funcWithReturnNonObj();
obj			// obj为funcWithReturnNonObj {x: 1},而不是构造函数返回的1
```

### 间接调用

`通过call()和apply()显式指定调用所需的this值,这种方式称为函数的间接调用`

```js
function func() {
    this.x = 1;
}
var o = {};
func.call(o);
o.x		// 返回为1
```

## 函数的实参和形参

### 可选形参

* 当调用函数的时候传入的实参比函数声明时指定的形参个数要少，剩下的形参都将设置为undefined值

  ```js
  function func(x, y, z) {
      console.log(x, y, z)
  }
  
  func(1)	// 打印结果为：1 undefined undefined,只传入了一个参数，函数执行的时候将匹配到形参x，剩余的y、z形参将设置为undefined，这是y和z就是可选形参了
  ```

  

### 可变长的实参列表：实参对象

* 问题：当调用函数传入的实参比函数声明时指定的形参个数要多时，没有办法直接获取未命名值的引用

* 解决方案：通过实参对象arguments可以取得所有实参

  ```js
  function f(x, y, z) {
      for(var i = 0; i < arguments.length; i++) {
          console.log(arguments[i]);
      }
  }
  f(1,2,3,4,5,6,7)
  f(1)
  ```

* arguments是一个类数组对象，不是一个真正的数组，其中arguments[0]指向的就是第一个新参

  ```js
  function f(x) {
      arguments[0] = 1;
      console.log(x);
  }
  f(2)		// 打印的结果为1
  ```

* 作用

  * arguments可以用来检测传入实参的个数是否正确

    ```js
    function f(x, y, z) {
        // 先检测实参个数是否正确
        if(arguments.length !== 3) {
            throw new Error('function f called with' + arguments.length + 'arguments'
                           + ', but it expects 3 arguments')
        }
        // 在执行函数的其他逻辑
    }
    ```

  * arguments可以用来让函数得以操作任意数量的实参

    ```js
    // 求实参的和
    function sum() {
        var data = 0;
        for(var index in arguments) {
            data += arguments[index]
        }
        return data;
    }
    sum(1,2,3,4,5,6,7,8) // 返回36
    sum(100, 200)		// 返回300
    ```

  * callee、caller属性

    * callee属性指向当前正在执行的函数（非严格模式下）
    * calller属性可以用来访问调用栈
    * 严格模式下callee和caller是不可读也不写的

    ```js
    // 阶乘计算函数
    var fatorial = function(x) {
        if(x <= 1) return 1;
        return x * arguments.callee(x-1)
    }
    fatorial(10) // 在匿名函数内部，递归调用自身，使用callee比较方便
    ```

## 作为值的函数

* 赋值给变量

  ```js
  var a = function() {} // 匿名函数赋值给变量a
  a()					  // a可以如此调用函数
  ```

* 赋值给数组元素

  ```js
  var a = [ function(x) { return x * x }, 20 ];
  a[0](a[1])		
  ```

* 自定义函数属性

  * 函数本身也是一个对象，可以自由的给函数定义属性

    ```js
    // 可以事先存储函数的形参默认值
    f.defaultParam = { x: 0, y: 0 };
    function f(x, y) {
       	x = x || f.defaultParam.x;
        y = y || f.defaultParam.y;
        return x + y;
    }
    f()			// 0
    ```

## 作为命名空间的函数

`当我们想定义一套可以重复使用的代码块时，代码块中的变量命名很容易与全局环境冲突，函数的存在允许我们去创建这类不污染全局环境的代码块`

```js
var x = 1;
function mymodule() {
    // 模块代码
    // 这个模块所使用的全部变量都是局部变量
    // 而不是污染全局命名空间
    var x = 2;
}
mymodule()		// 
x 		// x仍旧是1
```

**一种特殊用法：**函数在定义后可以马上调用

```js
(function() {
    
}())
// 这是很常用的一种用法
// 一般来说，这种写法是想定义一个马上执行且副作用小的代码块
```

## 闭包

### 一个例子来理解闭包

```js
// 这是一个用于计数的函数
function uniqueInteger() {
    if(this.count === undefined) {
        this.count = 0;
    }
    
    return this.count++;
}
uniqueInteger() 	// 每次执行方法，返回的count值增1
```

**存在什么问题？**

函数中的使用count变量是属于全局对象的，意味着我在注入恶意代码用于改变这个值

```js
count = 32;
uniqueInteger()		// 返回32，而不是从0开始
```

`uniqueInteger函数没有私有且从方法外侧无法改变的属性，导致基于此属性的操作容易受到外侧影响。`

**如何优化？**

```js
var uniqueInteger = (function() {
    var count = 0;
    return function() { return count++ }
}())

count = 32;
uniqueInteger()		// 返回0
```

`此时uniqueInteger变量指向的函数拥有私有的count属性，外侧无法改变`

`换句话说uniqueInteger变量指向的函数拥有自己的作用域`

**何为闭包？这就是闭包**

* 闭包是指能访问函数外作用域变量的函数。（语法角度）

  

### 更多例子

#### 计数器



```js
// 计数器
function count() {
    var n = 0; 	// 函数作用域中的变量
    return {
        count: function() { return n++ },
        reset: function() { n = 0; }	// 重置函数
    }
}

var a = count(), b = count();
a.count()		// 返回为0
b.count()		// 返回也为0，不受a对象的计时方法的影响
a.reset()		
a.count()		// 返回为0
b.count()		// 返回为1，说明由同一方法执行多次产生的闭包，各自的函数外作用域变量是相互独立的。
```



**思考一下，不用产生闭包的方式写出一个计时器？**

```js
var count = {
   	n: 0,
    count: function() { return this.n++ },
    reset: function() { this.n = 0 }
}
// 这种方式无法产生多个相互独立的计时器对象，只能沿用原型的计时器。
// 且容易报错，函数的调用是可以指定内部this的。
count.count.call(this)			// 返回NaN
```

#### getter、setter方法定义的闭包

```js
function counter() {
    var n = 0
    return {
        get count() { return n++ },
        set count(value) { n = value }
    }
}
var a = counter();
a.count			// 返回0
a.count = 2000	// 赋值count属性
a.count			// 返回2000
```

#### 利用闭包实现私有属性存取器方法

目的：给一个对象添加私有属性，并且提供存取方法(getXXX、setXXX)

```js
function addPrivateProp(o, propName, predicate) {
   	var value;
   	o['get' + propName] = function() { return value };
    o['set' + propName] = function(newVal) { 
        // 检验值是否合理
        if(predicate && !predicate(newVal)) {
            throw new Error("set" + propName + ":invalid value " + newVal)
        } else {
            value = newVal
        }
    }
}
var o = {};
o.setname("zsf")		// 属性赋值
o.getname()				// 属性取值
```

## 函数属性、方法和构造函数

### length属性

表示函数的形参个数

```js
function check(p1, p2, p3) {}
check.length		// 返回为3，因为在函数体中定义了三个形参
```

### prototype

表示原型对象，当该函数作为构造函数时创建的对象将继承prototype中的所有属性

### call()方法和apply方法

方法的间接调用，用于指定函数的调用上下文（this）

```js
Object.prototype.toString.call('test')	// 返回为"[object String]"
Object.prototype.toString.apply('test')	// 返回为"[object String]"
```

#### call和apply的区别

```js
f.call(o, 1, 2)
// 相当于
o.f = f;
o.f(1, 2);
delete o.f;

f.apply(o, [1, 2])
// 相当于
o.f = f;
o.f(1, 2)
delete o.f;
```

`两者的区别在于方法传参的方式不同，如代码所示`

**apply用到的实参可以是一个类数组对象，例如arguments**

### bind()方法

**ES5引入的语法糖**

主要作用：`将函数绑定至某个对象，内部的this不随环境变化而变化`

```js
function f() { console.log(this.x ++) }
var o = { x: 1 };
var fo = f.bind(o)
fo()			// 返回为1
```

**bind不仅仅可以绑定this，还可以依次绑定形参**

```js
function f(x, y) { return (this.x || 0) + (x || 0) + (y || 0) }
var f1 = f.bind({x: 1}, 2)
f1() 		// 返回3
f1(3)		// 返回6
f1(3, 4)	// 返回6
var f2 = f.bind({x: 1}, 2, 3)
f2()		// 返回6
f2(4)		// 返回6
```





**如何在ES3的环境下实现bind**

`闭包方式实现之`

```js
Function.prototype.easyBind = function(o) {
   	var self = this, boundArgs = arguments;
    return function() {
        var args = [], i;
        for(i = 1; i < boundArgs.length; i++) args.push(boundArgs[i]);
        for(i = 0; i < arguments.length; i++) args.push(arguments[i]);
        return self.apply(o, args)
    }
}
```

###toString()方法

函数对象的toString方法返回具体代码字符串

```js
function a () {}
a.toString();		// 返回"function a () {}"
var noNameFunc = function() {/** 注释**/};
noNameFunc.toStirng(); 	// 返回"function() {/** 注释**/}"，发现并没有名字,且保留了注释
```

### Function()构造函数

`函数还可以通过Function()构造方法来定义`

```js
var f = new Function("x", "y", "return x*y;");

// 等同于
var f = function(x, y) { return x*y }
```

### 可调用的对象

`函数都是可调用的，但是可调用的对象（callable object）不一定都是函数`

* RegExp对象本质上来说不是一个函数

## 函数式编程

### 高阶函数

`高阶函数（higher-order function）就是操作函数的函数，它接受一个或多个函数作为参数，并返回一个新函数`

```js
// 这个高阶函数接受f函数，返回新的函数
// 新的函数的执行结果是f函数执行结果的逻辑非
function not(f) {
    return function() {
        return !f.apply(this, arguments)
    }
}

// 判断是否为偶数
function even(n) {
    return n % 2 === 0
}

var odd = not(even);
odd(2)			// 返回false
```



























