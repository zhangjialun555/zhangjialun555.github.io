---
layout: post
title: "js原型和原型链"
date:   2020-12-05
tags: [note]
comments: true
toc: true
author: zhangjialun
---
带你重新认识js原型和原型链

<!-- more -->
## 开始

不管你做了多久的前端，原型和原型链一直是老生常谈的话题了，下面让我们深入浅出原型和原型链。

构造函数创建对象

```html
<script>
 function Person () {};
 var person = new Person();
 person.name = "lisa";
console.log(person.name)// lisa
</script>
```

Person 就是一个构造函数，我们使用new穿创建一个实person

### prototype

每个函数都有自己的prototype(原型)属性
每一个javascript对象(null)除外在创建的时候就会与之关联另外一个对象，这个对象就是我们所说的原型，每一个对象都会从原型继承属性。

```html
<script>
 function Person () {};
 Person.prototype.name = 'iu'
 const person1 = new Person()
 const person2 = new Person()
console.log(person1, person2)// iu iu
</script>
```

![构造函数](https://zhangjialun555.github.io/images/prototype/WechatIMG58.png)

Person(构造函数) 指向 Person.prototype(实例原型)

这其实就是我们所说原型继承，缺点就是子实例的时候，其他子实例也会被影响！

### proto

每一个对象都有自己的(除null)__proto__属性，这个属性会指向该对象的原型。
person.__proto__ === Person.prototype

![proto](https://zhangjialun555.github.io/images/prototype/WechatIMG59.png)

### cunstructor

每一个原型都有自己的constructor属性指向关联的构造函数，实例原型指向构造函数。

![cunstructor](https://zhangjialun555.github.io/images/prototype/WechatIMG60.png)

```html
<script>
 function Person () {};
 const person1 = new Person()
 console.log(person1.__proto__  === Person.prototype)//true
 console.log(Person === Person.prototype.constructor)//true
 // 顺便复习一个es5的方法，获取对象的原型
console.log(Object.getPrototype(person1) === Person.prototype)//true
</script>
```

### 实例与原型

```html
<script>
  function Person () {};
  Person.prototype.name = 'IU';
  let person1 = new Person();
  person1.name = 'lisa';
  console.log(person1.name);// lisa
  delete person1.name;
  console.log(person1.name);// IU
</script>
```

在这个例子中，我们给实例person1添加了属性，打印必然为lisa.
当我删除属性后，读取person.name，从person1对象查找不到就会访问person1的原型也就是person1__proto__。
也就是Person.prototype中查找，幸运的是我们找到了name为IU

### 原型与原型

```html
<script>
 const obj = new Object()
 obj.name = 'blackpink'
 console.log(obj.name)//blackpink
</script>
```

![原型与原型](https://zhangjialun555.github.io/images/prototype/WechatIMG61.png)

### 原型链

```html
<script>
  console.log(Object.prototype.__proto__ === null)//true
```

![原型链](https://zhangjialun555.github.io/images/prototype/WechatIMG62.png)

`
  javascript默认不会复制对象的属性。相反，javascript只是在两个对象之间创建一个关联，这样，一个对象就可以通过委托访问另外一个对象的属性和函数。
所以与其叫继承，委托的说法反而更准确些。
`

