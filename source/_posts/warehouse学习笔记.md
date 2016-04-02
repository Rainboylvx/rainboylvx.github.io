



## 入门

 - [git地址](https://github.com/tommy351/warehouse)
 - [api文档](http://tommy351.github.io/warehouse/)

warehouse 是一种使用JSON来存数据的一种本地数据库,他的模型应该和`mongoose`一样,应该只要理解了他的模型就下面就是不很难了,都是函数的应该

数据库db() -- > model db里的集合,在集合里可以
Schema 数据骨架,定义了可以存,操作哪些类型的值,同时还可以有方法,可以通过schema来生成model,Schema可以定义自己的方法,这太有有了

model ,关联到具体的哪一个集合+Schema 生的成`模型`,可以CRUD

Entity 可以认为一份Docment,可以用model+具体数据来生成Entity,根据某些条件来操作里的数据,最后Entity.save()

```
var Database = require('warehouse');
var db = new Database();

var Post = db.model('posts', {
  title: String,
  created: {type: Date, default: Date.now}
});

Post.insert({
  title: 'Hello world'
}).then(function(post){
  console.log(post);
});
```




