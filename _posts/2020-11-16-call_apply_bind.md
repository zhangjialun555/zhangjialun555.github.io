---
layout: post
title: "call&&apply&&bind"
date:   2020-11-16
tags: [note]
comments: true
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
