---
title: javascript笔记
date:  2016-04-12 15:22
categories:
    - 学习笔记
tags:
    - javascript
---


## javascript中的原型

!!!如果调用函数的时候在前面加上了`new`,其this变量将指向一个新对象,该对象会自动返回,以这种方式创建对象的函数称为**构造函数**.

!!!虽然对象可以共享其原型的属性,但是这种共享是单向的:原型的属性影响对象,改变对象却永远不会影响到原型.

rabbit对象基于与Rabbit构造函数相关的原型.可以使用构造函数的prototype属性访问该原型.


定义的每个函数都会自动获取一个prototype属性,这个属性有一个该函数的对象:原型

原型对象是由函数的构造函数创建，它所拥有的属性能被所有对象共享，初始时原型对象指向一个Object对象

原型有一个constructor属性,反指向其当前所属的函数

```
        <Script Language="JavaScript">
function Rabbit(adj){
    this.adj =adj;
    this.speaker = function(line){
        alert("the "+this.adj+" rabit say '"+line+"'");
    };
}

Rabbit.prototype.teeh="small";

var killerRabbit = new Rabbit("killer");
killerRabbit.speaker("GRAAA");

alert(killerRabbit.teeh);

killerRabbit.teeh = "long,sharp,and bloody";
alert(killerRabbit.teeh);

        </script>
```
