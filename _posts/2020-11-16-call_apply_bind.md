---
layout: post
title: "call&&apply&&bind"
date:   2020-11-16
tags: [note]
comments: true
toc: true
author: zhangjialun
---
解析this call apply bind
用js实现

<!-- more -->
## 解析this

### this指向

- this 永远指向最后调用它的那个对象 永远指向最后调用它的那个对象 永远指向最后调用它的那个对象

```html
<script>
    var name = "windowsName";
    function a() {
        var name = "Cherry";

        console.log(this.name);          // windowsName

        console.log("inner:" + this);    // inner: Window
    }
    a();
    console.log("outer:" + this)         // outer: Window
</script>
```

```html
<script>
    var name = "windowsName";
    var a = {
        name: "Cherry",
        fn : function () {
            console.log(this.name);      // Cherry
        }
    }
    a.fn();
</script>
```

```html
<script>
var name = "windowsName";
    var a = {
        name: "Cherry",
        fn : function () {
            console.log(this.name);      // Cherry
        }
    }
    window.a.fn();
</script>
```

```html
<script>
    var name = "windowsName";
    var a = {
        // name: "Cherry",
        fn : function () {
            console.log(this.name);      // undefined
        }
    }
    window.a.fn();
</script>
```

```html
<script>
    var name = "windowsName";
    var a = {
        name : null,
        // name: "Cherry",
        fn : function () {
            console.log(this.name);      // windowsName
        }
    }

    var f = a.fn;
    f();
</script>
```

```html
<script>
    var name = "windowsName";

    function fn() {
        var name = 'Cherry';
        innerFunction();
        function innerFunction() {
            console.log(this.name);      // windowsName
        }
    }

    fn()
</script>
```

### 改变this指向

- 使用箭头函数

- 在函数中使用 _this = this

- call apply bind

- new 实例化一个对象

#### 箭头函数

- 箭头函数中的this指向函数定义时的this,非执行时

- 箭头函数无this绑定，必须通过查找作用域链来决定其值，如果箭头函数被非箭头函数所包含，则this指向的时最近一层非箭头函数的this,否则this为undefined

```html
<script>
    var name = "windowsName";

    var a = {
        name : "Cherry",

        func1: function () {
            console.log(this.name)
        },

        func2: function () {
            setTimeout( () => {
                this.func1()
            },100);
        }

    };

    a.func2()     // Cherry
</script>
```

#### 在函数内部使用_this = this

- 如果不实用es6,那么这种方式应该是最简单最容易理解的了，我们是先将调用这个函数的对象保存在变量_this中，然乎在函数中都使用这个_this，这样this就不会改变了。

```html
<script>
var name = "windowsName"
var a = {
  name = "cherry",
  func1:function() {
    console.log(this.name)
  },
  func2:function () {
    var _this = this
    setTimeout(function () {
      this.func1()
    },100)
  }
}
a.fun2() // cherry
</script>
```

#### 使用call、apply、bind改变this

- 使用call apply bind

```html
<script>
var name = "windowsName"
var a = {
  name = "cherry",
  func1:function() {
    console.log(this.name)
  },
  func2:function () {
    var _this = this
    setTimeout(function () {
      this.func1()// 理解为执行时绑定this到a函数体内部
    }.apply(a),100)// apply 绑定参数形式为a.call(b,[list1,list2])
    // }.call(a),100)// call 绑定参数形式为a.call(b,list1,list2)
    // }.bind(a)(),100)// bind 返回一个新函数 需要手动执行
  }
}
a.fun2() // cherry
</script>
```

 在MDN中apply定义如下：

`
apply方法调用一个函数，其具有一个指定的this值，以及作为一个数组（或累数组的对象）提供的参数
`
语法：

`
fun.apply(thisArg,[argsArr])
`

- thisArg：在 fun 函数运行时指定的 this 值。需要注意的是，指定的 this 值并不一定是该函数执行时真正的 this 值，如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动指向全局对象（浏览器中就是window对象），同时值为原始值（数字，字符串，布尔值）的 this 会指向该原始值的自动包装对象。

- argsArray：一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 fun 函数。如果该参数的值为null 或 undefined，则表示不需要传入任何参数。从ECMAScript 5 开始可以使用类数组对象。浏览器兼容性请参阅本文底部内容。

call 只是与apply传参不同，语法：

`
func.call(thisArg,[, arg1[, arg2[, ...]]])
`

```html
<script>
var a = {
  name : "cherry",
  fn: function (a, b) {
    console.log(a + b)
  }
}
var b = a.fn()
b.apply(a,[1,2]) //3
// b.call(a,1,2) // 3
</script>
```

- MDN中bind解释为：

`
bind()方法创建一个新的函数, 当被调用时，将其this关键字设置为提供的值，在调用新函数时，在任何提供之前提供一个给定的参数序列。
`

```html
<script>
var a = {
  name : "cherry",
  fn: function (a, b) {
    console.log(a + b)
  }
}
var b = a.fn()
b.bind(a,1,2)() //3
</script>
```
