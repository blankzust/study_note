---

---

# statement

* 语句就是JavaScript整句或命令

## 表达式语句

<!--由运算符组成的具有副作用的语句-->

~~~js
var a = 1
~~~

<!--不一定存在左值,由几元运算符决定-->

~~~js
delete prop.xxx
~~~

<!--函数调用从形式上来说是一种表达式，但是我们认为他们也是语句-->

<!--因为函数在调用的时候大概率是产生了副作用-->

## 复合语句

<!--格式-->

~~~js
{
    statement1;
    statement2;
    statement3;
    ...
}
~~~

<!--if循环、for语句等后面常跟复合语句-->

<!--用来表示这些子语句依次执行，并且作为前面语句的后一句语句执行-->

### 注意点

** 复合语句中并不享有独有的作用域 **

~~~js
var a = 1;
{
    var a = 2;
}
a
> 2
~~~

<!--上诉代码中的复合语句中定义的a变量就是全局变量-->

## 空语句

** 空语句指的是包含0条语句的语句 **

## 声明语句

** 定义标识符并且给其赋值的语句 **

### var

作用：`var语句用来声明一个或多个变量`

语句格式：

```js
var name_1[= value_1][,...,name_n[= value_n]]
```

声明提前：变量申明语句会被“提前”至脚本或者函数的顶部，但是初始化的操作（赋值）则还在原来var语句定义的位置执行。在申明语句之前的上下文环境中变量的值是undefined。

```js
var b = a + '';
var a = 1;
console.log(b)
```
------

等同于

----

```js
var b;
var a;
b = a + '';
a = 1;
console.log(b);
```

### function

作用：`关键字function用来定义函数`

语句格式：

```js
function funcname([arg1[,arg2[...,argn]]]) {
    statements
}
```

**声明提前**：函数声明语句将被“提前”至脚本或者函数的顶部，连带初始化操作会一并提前执行；

比较以下两端程序的输出结果

```js
var temp = func1 + '';
function func1() {};
console.log(temp)
```

----

```js
var temp = func1 + '';
var func1 = function() {};
console.log(temp);
```

**注意点**：

* 函数内部可以定义新函数，但是必须方法的顶层，即方法的定义不得存在与判断语句、循环语句中；

## 条件语句

### if

```js
if(expression)
    statement
[
else if(expression1)
    statement_1
...
]
[else statement_n]
```

### switch

```js
switch(expression) {
    case value_1:
        statements_1
        [break;]
    ...
    case value_n:
        statements_n
        [break;]
    default:
        statements_default
}
```

## 循环

### while

### do while

### for

### for/in

语句格式：

```js
for(variable in object)
    statement
```

作用：`将会遍历object中的可枚举属性，没一次循环将对in的左值进行一次赋值操作`

**扩展点**

* 可枚举属性和不可枚举属性

  代码中定义的属性和方法大概率是可枚举属性；

  继承于其他对象的自定义属性；

  JavaScript语言核心所定义的内置方法是不可枚举的；eg.`toString、valueOf`等方法；

  很多内置对象的属性也是不可枚举的；

* 如何自定义一个不可枚举属性？

  ES5定义了一个名为“属性描述符”的对象

  ```js
  var o = {};
  Object.defineProperty(prop, 'x', { value: 1, enumerable: false })
  console.log(prop.x); // =》1
  for(var a of o) { console.log(a) } // 无输出
  ```

* 属性的枚举顺序：ES规范没有指定属性的枚举顺序，但是大部分的浏览器都是按照属性定义的先后顺序来枚举简单对象的属性的

## 跳转

### 标签语句

```js
identifier: statement
```

break、continue语句后可直接跟标签名，用来跳转到指定标签的代码块的末尾或起点；

## 其他语句

### with

作用：`临时扩展作用域链`

```js
with(document.forms[0]) {
    name.value = "";
}
```

等同于

```js
document.forms[0].name.value = ""
```

### debugger

作用：`当浏览器处于调试模式时，运行至debugger处会暂停，方便调试`

### "use strict"

作用：`ES5引入的指令，并不是语句`

**扩展点**

* 指令与语句的区别

  1. 指令仅仅是一个包含一个特殊字符串直接量的表达式
  2. 指令只能出现在脚本代码的开始或者函数体的开始、任何实体语句之前

* 严格模式与非严格模式的区别

  [见链接](https://blog.csdn.net/weiyongliang_813/article/details/50157279)

