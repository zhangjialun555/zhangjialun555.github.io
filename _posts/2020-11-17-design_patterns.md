---
layout: post
title: "设计模式"
date:   2020-11-17
tags: [note]
comments: true
author: zhangjialun
---
1. 设计模式类似于前人总结出来的数学公式

2. 设计模式的核心思想---封装变化：将变与不变分离，确保变化的灵活性，不变化的稳定性。

3. 设计模式可分三类：创建型，行为型，结构型

<!-- more -->
![展示](https://zhangjialun555.github.io/images/design_patterns/544b34db.jpg)

### 创建型：工厂模式

- 就是将创建的对象单独传参，为实现无限制传参

文中举例： 是一个员工录入系统，其中共性的部分是 每个员工都有姓名、年龄、身份所以就可以给他们都封装成一个类

```html
<script>
  function User(name, age, career) {
    this.name = name;
    this.age = age;
    this.career = career;
  }        // outer: Window
</script>
```

但是呢，老板说这个系统录入的信息也太简单了，程序员和产品经理之间的区别一个简单的 career 字段怎么能说得清？我要求这个系统具备给不同工种分配职责说明的功能。也就是说，要给每个工种的用户加上一个个性化的字段，来描述他们的工作内容。
所以员工的共性被拆离了，想要实现的话，可能就要封装出多个类，比如

```html
<script>
  function Coder(name, age) {
    this.name = name;
    this.age = age;
    this.career = "coder";
    this.work = ["写代码", "写系分", "修Bug"];
}
function ProductManager(name, age) {
    this.name = name;
    this.age = age;
    this.career = "product manager";
    this.work = ["订会议室", "写PRD", "催更"];
}
</script>
```

这样公司有多少个工种就会有多少类，所以这样明显是不可以的。回头想一下，所谓的work其实也是每个员工都有的共性，只不过 work 字段需要随 career 字段取值的不同而改变，所以我们改造一下 User 类，然后把这个承载了共性的 User 类和个性化的逻辑判断写入同一个函数

```html
<script>
  function Coder(name, age,career,work) {
    this.name = name
    this.age = age
    this.career = career
    this.work = work
  }
  function Factory(name,age,career) {
    let work
    switch (career) {
      case 'coder':
        work = ['写代码','写系分', '修Bug']
        break
      case 'product manager':
        work = ["订会议室", "写PRD", "催更"]
        break
      case 'boss':
        work = ['喝茶', '看报', '见客户']
        break
      case 'xxx':
        ///
    }
    return new User(name, age, career, work)
  }
</script>
```

#### 抽象工厂

 想要生产一台 FakeStar 手机时只需要这样做

```html
<script>
    // 这是我的手机
    const myPhone = new FakeStarFactory();
    // 让它拥有操作系统
    const myOS = myPhone.createOS();
    // 让它拥有硬件
    const myHardWare = myPhone.createHardWare();
    // 启动操作系统(输出‘我会用安卓的方式去操作硬件’)
    myOS.controlHardWare();
    // 唤醒硬件(输出‘我会用高通的方式去运转’)
    myHardWare.operateByOrder();
</script>
```

假如有一天，FakeStar 过气了，我们需要产出一款新机投入市场，这时候怎么办？我们是不是不需要对抽象工厂 MobilePhoneFactory 做任何修改，只需要拓展它的种类:

```html
<script>
    class newStarFactory extends MobilePhoneFactory {
       createOS() {
          // 操作系统实现代码
        }
        createHardWare() {
          // 硬件实现代码
        }
    }
</script>
```

这样一来，对原有的系统不会造成潜在的任何影响，所谓的“对拓展开放，对修改封闭”，就这么圆满实现了，前面我们所说的要抽象产品类也是同样的道理。

#### 总结

抽象工厂和简单工厂的异同：

共同点：都在尝试去分离一个系统中变与不变的部分
不同点：场景复杂度。简单工厂模式，处理的对象是类，共性容易抽离，逻辑本身比较简单。
抽象工厂本质上处理的其实也是类，但是是一帮非常棘手、繁杂的类这些类中不仅能划分出门派， 还能划分出等级，同时存在着千变万化的扩展可能性——这使得我们必须对共性作更特别的处理、使用抽象类去降低扩展的成本，同时需要对类的性质作划分。 于是有了这样的四个关键角色：

1. 抽象工厂 (抽象类，它不能用于生成具体事例): 用于声明最终目标产品的共性。在一个系统里，抽象工厂可以有多个（大家可以想象我们的手机厂后来被一个更大的厂收购了，这个厂里除了手机抽象类，还有平板、游戏机抽象类等等），每一个抽象工厂对应的这一类的产品，被称为“产品族”。

2. 具体工厂 (用于生成产品族里的一个具体的产品): 继承自抽象工厂、实现了抽象工厂里声明的那些方法，用于创建具体的产品的类。

3. 抽象产品（抽象类，它不能被用于生成具体实例）: 上面我们看到，具体工厂里实现的接口，会依赖一些类，这些类对应到各种各样的具体的细粒度产品（比如操作系统、硬件等），这些具体产品类的共性各自抽离，便对应到了各自的抽象产品类。

4. 具体产品（用于生成产品族里的一个具体的产品所依赖的更细粒度的产品）: 比如我们上文中具体的一种操作系统、或具体的一种硬件等。

### 单例模式

只允许存在一个实例的模式

首先我们知道通过类模式，可创建多个实例

```html
<script>
  class Single {}
  const s1 = new Single()
  const s2 = new Single()
  console.log(s1===s2)///false
</script>
```

两个实例之间没有任何关系，就是两个独立的对象，各占一块内存空间

单例模式想要做到的是，不管我们尝试去创建多少次，它都只给你返回第一次所创建的那唯一的一个实例。

所以我们就要构造函数具备判断自己是否已经创建过一个实例的能力

我们现在把这段判断逻辑写成一个静态方法(其实也可以直接写入构造函数的函数体里）：

```html
<script>
  class Single {
    static GetInstance () {
      // 判断是否已经new过一个实例
      if (!SingleDog.instance) {
        //若实例不存在，创建实例
        SingleDog.instance = new SingleDog()
      }
      //如果实例存在，则返回实例
      return SingleDog.instance
    }
  }
  const s1 = new Single.GetInstance()
  const s2 = new Single.GetInstance()
  console.log(s1 === s1)//true
</script>
```

单例模式的典型应用 Vuex => 一个Vue实例 只能对应一个Store

为什么这么做呢？

Vue 中的组件是相互独立的，组件间的通信也很简单，但是组件非常多且嵌套层级很深，逻辑较为复杂的时候，这时如果还用原有的通信方式就会变得非常难以维护，很容易发生那种牵一发动全身的问题。Vuex就是为了解决这种场景出现，它的出现也大大增强了Vue的健壮性，不再是那个只能构建轻量型的应用的框架了。而他的做法就是把可以共享的数据放在全局，供组件们使用。为了能够让Store中的数据以一种可预测的方式发生变化，那么必然要保证Store中的数据的唯一性 => 单例模式的应用

### 观察者模式

`
观察者模式定义了一种一对多的依赖关系，让多个观察者对象同时监听某一个目标对象，当这个目标对象的状态发生变化时，会通知所有观察者对象，使它们能够自动更新。
`

在观察者模式中，至少应该有两个关键角色是一定要出现的——发布者和订阅者。用面向对象的方式表达的话，那就是要有两个类。
发布者的类具备的基本功能：

- 增加订阅者
- 移除订阅者
- 通知订阅者

```html
<script>
  class Publisher {
    constructor () {
      this.observers = []
      console.log("publisher is created")
    }
    add(observer) {
      this.observers.push(observer)
      console.log("add observer")
    }
    remove(observer) {
      this.observers.forEach((item,index) => {
        if (item === observer) {
          this.observers.split(1, index)
        }
      })
      console.log("remove observer")
    }
    notify () {
      console.log("notify observer")
      this.observers.forEach(item => {
        item.update(this)
      })
    }
  }
</script>
```

订阅者的功能：

- 被通知

- 去执行（本质上是接受发布者的调用，这步我们在 Publisher 中已经做了）

既然我们在 Publisher 中做的是方法调用，那么我们在订阅者类里要做的就是方法的定义

```html
<script>
  class Observer {
    constructor() {
      console.log("observer is created");
    }
    update(publish) {
      console.log("observer is notified");
    }
  }
</script>
```

插入一段理解vue双向数据绑定的原理理解：

`
数据驱动：vue.js一个核心的思想是数据驱动。所谓的数据驱动是指视图是由数据驱动生成的，对视图的修改，不会直接操作DOM,而是通过修改数据。相比于传统的前端开发，如使用jq等前端库直接修改DOM，大大简化了代码量，特别是当交互复杂的时候，只关心数据的修改会让代码的逻辑变得非常清晰，因为DOM变成了数据的映射，我们所有的逻辑都是对数据的修改，而不用触碰DOM，这样的代码非常有利于维护。
`
在MVVM中，M(model)---代表JavaScript  Objects，V(view)---DOM也就是UI，VM(ViewModel)----代表Vue实例对象（在该对象中有Directives和DOM Listeners）

在vue.js里面只需要改变数据，Vue.js通过Directives指令去对DOM做封装，当数据发生变化，会通知指令去修改对应的DOM，数据驱动DOM的变化，DOM是数据的一种自然的映射。vue.js还会对View操作做一些监听（DOM Listener），当我们修改视图的时候，vue.js监听到这些变化，从而改变数据。这样就形成了数据的双向绑定。

- Vue实现数据双向绑定的原理：

![双向数据绑定](https://zhangjialun555.github.io/images/design_patterns/20180704102901151.png)

如图所示，new Vue一个实例对象a，其中有一个属性a.b,那么在实例化的过程中，通过Object.defineProperty()会对a.b添加getter和setter，同时vue.js会对模版做编译，解析生成一个指令对象（这里是v-text）指令，每个指令对象都会关联watcher,当对a.b求值时，就会触发他的getter，当修改a.b时就会触发他的setter，同时会通知被关联的watcher，然后watcher会再次对a.b求值，计算对比新旧值，当值改变了，watcher就会通知到指令，调用update(),由于指定是对DOM的封装，所以会调用DOM的原生方法去更新时图，这样就完成了数据改变到视图改变的一个自动化过程。
同时vue还会对DOM做事件监听，如果DOM发生变化，vue监听到了，就会修改相应的data.

#### 总结

直接上总结吧，详细的案例有待研究，不过目前基本已经捋清楚vue双向数据绑定的基本流程

1. 通过observe()函数里的Object.defineProperty()监听UI的变化，从而得到数据的变化；
再通过compile()函数监听绑定该数据的DOM节点，当数据变化的时候就会通知这些节点更新数据，从而得到UI的变化

2. observe()函数实现监听UI的变化，从而得到数据的更新
compile()函数监听数据变化，然后将数据传送到绑定的DOM节点上显示。