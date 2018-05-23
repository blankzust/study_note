# 数组

数组内容较为简单，故只摘记一些关键知识点

## 创建数组

**注意点**

1. ```js
   var undefs = [,,];	
   undefs.length;		// 返回2
   // 数组直接量的语法允许有可选的结尾的逗号，故[,,]只有两个元素而非三个
   ```

## 数组元素的读写

使用[]操作符来访问数组中的一个元素。

**注意点**

* 可以使用非整数来索引数组

  ```js
  var a = [];
  a[-0.1] = 1;
  a["1000"] = 2;	// 字符串的索引会先尝试转换为数字，若成功转换，可能会产生大量的多余空间
  a[1.000] = 3;	// 浮点数也将尝试转换为数字
  a // 返回为：[empty, 3, empty × 998, 2, -0.1: 1]
  ```

* 数组索引仅仅是对象属性名的一种特殊类型，这意味着JavaScript数组没有“越界”错误的概念

* 数组是对象，数组可以定义元素的getter和setter方法

**疑问点**

* 如果一个数组继承了元素或者使用了元素的getter和setter方法，你应该期望它使用非优化的代码路径：访问这种数组的元素的时间会与常规对象属性的查找时间相近。（不能理解这里说的非优化的代码路径）



## 稀疏数组

稀疏数组就是包含从0开始的**不连续索引**的数组

```js
a = new Array(5) // 数组没有元素，但是a.length是5，这是一个稀疏数组
a = [];
a[1000] = 0;	 // 指定数组的所有值大于当前数组的长度，可以创建稀疏数组
```

## 数组方法

* join

  

  ```js
  var a = ['1', '2', '3']
  a.join();		// "1,2,3"
  a.join("-");	// "1-2-3"
  
  var b = [1];
  b.length = 5;	// 稀疏数组
  b.join();		// "1,,,,"
  
  ```

* reverse

  ```js
  var a = [1,2,3];
  a.reverse().join(); // 输出“3,2,1”，并且现在的a为[3,2,1]
  ```

* sort

  ```js
  var a = ['banana', 'apple', 'cherry'];
  a.sort();
  a.join();		//返回'apple,banana,cherry'
  a.sort(function(pre, next) {
      return pre - next;
  })
  ```

  ...

## 类数组对象

`同样具有length属性且属性名为数组的对象成为类数组对象`

```json
{
    1: 'a',
    2: 'b',
    3: 'c',
    length: 3
}
```

### 如何区分类数组对象和数组

* 使用Array.isArray方法区分

  ```js
  var arrayLike = { 1: 'a', 2: 'b', 3: 'c', length: 3 };
  var array = ['a', 'b', 'c'];
  Array.isArray(arrayLike)	// false
  Array.isArray(array)		// true
  ```

  

* 从原型链角度区分

  ```js
  array.__proto__ === Array.prototype;	// true
  arrayLike.__proto__ === Array.prototype;	// false
  
  // 调用Object原型对象的toString方法，并绑定此方法的调用者为参数o
  function isArray(o) {
      return typeof o === 'object' && Object.prototype.toString.call(o) === '[object Array]'
  }
  ```

* instance操作数判断

  ```js
  arrayLike instanceOf Array // false
  array instanceOf Array // true
  ```

  