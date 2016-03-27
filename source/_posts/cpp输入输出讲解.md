---
title: C++输入输出讲解
date: 2016-03-27 08:37
categories: 课件
tags: 
 - C++教程
---

##说明 

我们使用的是C++语言,C++语言是对C语言的拓展,主要加上的是面对对象的部分,它的输出的风格是`cin`和`cout`.C++语言为了保持对C语言的兼容,同时也有C风格的的输入输出`scanf`,`printf`.

在具体的学习中,我是更倾向使用`scanf`,`printf`,这里的原因和风格有一定关系,但是最重要的是:**在以完成题目的为前提下,选一个简单的来用,keep it simple and stupid**

下面我会把这两种风格都说一下:




## C++中C风格的的输入输出

### 先来一点预备知识

前面的学习我们已经知道`` `A` ``,这样被单引号括表示的是字符,`` "hello world" ``这样被又引号括起来是是字符串,所谓的`字符串`就是`字符`的组合,这样可以产生一些有意义的语句了.

我们平常写的代码,想要输出的东西就是一串字符,输出到console上(就是那个黑色的框框).那我们怎么用C风格来输出呢?

```
printf("hello word!");
```

当然我们要引入C风格头文件`#include <cstdio>`.上面的是最简单的输出,不好的地方就是每一次的输出都一样,如果我们想要让输出按我们的意思输出怎么办?比较输出`int a=10;`这种变量的值怎么办?


### C风格 格式化输出 printf

printf调用:

<center> ** printf("格式化字符串",参数表)</center>

格式化字符:

<center>%[标志][输出最小宽度][.精度][长度]格式化字符</center>

更标准一点的说法:

<center>%[flags][width][.prec][F|N|h|i]type</center>


简单的例子:

```
printf("%1.2f",11.11111)

```

得到的答案是`1.11`,因为我们规定了输出的width是1,prec是2,那余的东西就会被丢掉.下面看表格:


| flags | 名称   | 作用                                                                                               |
|-------|--------|----------------------------------------------------------------------------------------------------|
| -     | 左对齐 | 右边填充空格(默认右对齐)                                                                           |
| +     | 加号   | 在数字前增加符号 + 或 -                                                                            |
| 0     | 数字零 | 将输出的前面补上0，直到占满指定列宽为止（不可以搭配使用“-”）                                       |
| 　    | 空格   | 输出值为正时加上空格，为负时加上负号                                                               |
| #     | 井号   | type是o、x、X时，增加前缀0、0x、0X,type是e、E、f、g、G时，一定使用小数点,type是g、G时，尾部的0保留 |


| type | 对应的数据类型 | 作用                                     |
|------|----------------|------------------------------------------|
| d    | int            | 接受整数值并将它表示为有符号的十进制整数 |
| lld  | long long      | 同上,但是表示64bit的long long            |
| x    | unsiged int    | 小写16进制                               |
| X    | unsiged int    | 大写16进制                               |
| o    | unsiged int    | 8进制                                    |
| u    | unsiged int    | 无符号10进制                             |
| f    | float          | 单精度浮点数                             |
| lf   | double         | 双精度浮点数                             |
| s    | char *         | 字符串                                   |


width:
是一个可选的指定最小值字段宽度的十进制数字字符串。如果转换值字符少于字段宽度，该字段将从左到右按指定的字段宽度填充。如果指定了左边调整选项，字段将在右边填充。如果转换结果宽于字段宽度，将扩展该字段以包含转换后的结果。不会发生截断。然而，小的精度可能导致在右边发生截断。

prec:
用于控制小数点后面的位数，取值和含义如下：缺省精度0,其它精度n(1,2,3,4...)

| 转义字符 | 作用                       |
|----------|----------------------------|
| \n       | 换行                       |
| \\       | 一个\                      |
| \'       | 一个'                      |
| \"       | 一个"                      |
| \xhh     | hh二位的16进制所代表的字符 |


 > **注意:** 上面说了那么多,是不是都要记住,不要,我们只要知道有这些东西就行了,具休只要下面的代码有看懂就行


#### 代码举例

```
#include <cstdio>
int main(){
    double a=1123.123456;
    int b =12;
    printf("%6.2f\n",a);
    printf("%d\n",b);
    printf("%4d\n",b);
    printf("%-4d\n",b);
    printf("%04d\n",b);
    printf("%4.4d\n",b);
    printf("my favrite num is :%d\n",9);
    printf("%03d %c,%f hh %s",12,'A',1.1,"RAINBOY");
    return 0;
}
```

结果是

```
1123.12
12
  12
12
0012
0012
my favrite num is :9
012 A,1.100000 hh RAINBOY
```


### scanf 的使用
 scanf() 是从标准输入流stdio (标准输入设备，一般是键盘)中读内容的通用子程序，可以说明的格式读入多个字符，并保存在对应地址的变量中。[

 调用方式:

<center>scanf("参数表",变量地址)</center>

 简单的来说就是从console里读取,下面几个问题我们要知道:

 - %d读取的时候会把不可见的字符略去
 - 你在console输入数据的时候,按下回车的时候,才把数据提交
 - 定 %d%d%d 之间最好不要有其它东西


#### 代码

```
#include<stdio>
int main(void)
{
int a,b,c;
printf("输入a,b,c\n");
scanf("%d%d%d",&a,&b,&c);
printf("a=%d,b=%d,c=%d\n",a,b,c);
return 0;
}
```

基本会写上面的这个代码就会能写90%以上的题目了,如果在写题目真的遇到了怎么办?看这个[百度百科](http://baike.baidu.com/link?url=1f3edcVHlc0ey2QsxEHi9zDDB8HwwYWmXMUgP0HZM1CDDE-PD5Yr8mUV5dTDlJAI8kaiu2hDHACfLOh3avWQS_)

下面的这些要撑握:

1.读取到文件地末尾:

2.读取一行字符:

3.读取一行数字:

## cin cout 的使用

要包含头文件:`<iostream>`,要写这句话:`using namespace std;`


cout语句的一般格式为：`cout<<表达式1<<表达式2<<……<<表达式n;`

cin语句的一般格式为：`cin>>变量1>>变量2>>……>>变量n;`

这里说一下对理解:把cout想像成cout 想象成console, `<<`表示一个流向,会把数据流向cout,而cout就是console,现时`<<`操作完后会返回cout,所以可以写成一个长链,cin同理,只不过把cin当成键盘.

与printf相比,如果我想输出一行话,只要写一个长链串起来就可以了.

```
cout << "hello word" << 10 << ' ' << endl;
```

### cin cout 控制符

`#include <iomanip>`


| cout 控制         |                        |
| ----------------- | ---------------------- |
| dec               | %d                     |
| hex               | %x                     |
| oct               | %o                     |
| setfill('c')      | 填充字符为c            |
| setw(n)           | 宽度为n                |
| setprecision(b)   | 浮点数小数部分长度为n  |

#### 代码

```
/*一个按进制输出的例子*/

#include<iostream.h>

void main()

{

int x=30, y=300, z=1024;

cout<<x<<' '<<y<<' '<<z<<endl; //按十进制输出

cout.unsetf(ios::dec ); //取消十进制输出设置

cout.setf(ios::showbase | ios::uppercase); //设置基指示符输出和数值中的字母大写输出

cout<<x<<' '<<y<<' '<<z<<endl;

cout.unsetf(ios::showbase | ios::uppercase); //取消基指示符输出和数值中的字母大写输出

cout.setf(ios::oct); //设置为八进制输出,此设置不取消一直有效

cout<<x<<' '<<y<<' '<<z<<endl; //按八进制输出

cout.setf(ios::showbase | ios::uppercase); //设置基指示符输出和数值中的字母大写输出

cout<<x<<' '<<y<<' '<<z<<endl;

cout.unsetf(ios::showbase | ios::uppercase); //取消基指示符输出和数值中的字母大写输出

cout.unsetf(ios::oct); //取消八进制输出设置，恢复按十进制输出

cout.setf(ios::hex); //设置为十六进制输出

cout<<x<<' '<<y<<' '<<z<<endl;

cout.setf(ios::showbase | ios::uppercase); //设置基指示符输出和数值中的字母大写输出

cout<<x<<' '<<y<<' '<<z<<endl;

cout.unsetf(ios::showbase | ios::uppercase); //取消基指示符输出和数值中的字母大写输出

cout.unsetf(ios::hex); //取消十六进制输出设置，恢复按十进制输出

cout<<x<<' '<<y<<' '<<z<<endl;

} 
```


##FAQ:算法竞赛的时候用cin cout输入输出比用scanf printf慢多少？

 - https://www.zhihu.com/question/27831271/answer/38379464
 - https://www.byvoid.com/blog/fast-readfile

## 要善用网络

