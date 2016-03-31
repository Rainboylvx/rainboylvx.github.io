---
title: Graphviz软件的学习
date:
categories: 学习笔记
---


## Graphviz 一直找的画图软件

[官网](http://www.graphviz.org/)

从官网下载,对应的版本安装

学习资源:
 - [利用Graphviz 画结构图](http://www.cnblogs.com/sld666666/archive/2010/06/25/1765510.html)
 - [DOT语言 GUIDE](http://wenku.baidu.com/view/6ecab70203d8ce2f00662341.html)
 - [(转)使用graphviz绘制流程图](http://icodeit.org/2012/01/%E4%BD%BF%E7%94%A8graphviz%E7%BB%98%E5%88%B6%E6%B5%81%E7%A8%8B%E5%9B%BE/)

## 入门

digraph 是有向图 边 `->`
graph 是无向图 边 `--`

一个简单的图:

```
digraph G {
    1->2;
    2->1;
    3->4;
    2->4;
}
```

点的默认属性:

 >
 shape = ellipse,width=7.5,height=.5
 style 线的形状
 每个点有一个单独的label
 box,circle,record ,plaintext 默认形状

例子:
```
digraph G {
    size ="4.4";
    main [shape =box];
    main -> parse[weight=8];
    parse -> execute;
    main -> init[style=dotted];
    main -> cleanup;
    execute -> {make_sring;printf};
    init -> make_sring;
    edge [color=red];
    main -> printf[style=bold,label='100 times'];
    make_sring[label="make a\nstring"];
    node [shape=box,style=filled,color=".7 .3 1.0"];
    execute->compare;
}

```



### 语法dataSheet

| shape     | node的类型 |
| record    | 记录       |
| color     |            |
| fillcolor | 填充色     |
| label     | 标签名     |
| fontsize  | fontsize   |
| fontname  | fontname   |


| style  | 边的类型 |
|--------|----------|
| dashed | 虚线     |
| solid  | solid    |
| filled | 实线     |



色表

 1. 先定义点,后定义边
 2. 子图的名称必须以cluster
 3. 节点的label属性支持类似于HTML语言中的TABLE形式的定义
 4. rankdir = LR; TB ,RL BT 从左到右
 5. arrowhead 每条边箭头的形状:`both`,`none`,`back`,`forward(default)`
 6. shape ellipse,box,circle,record ,plaintext 默认形状
