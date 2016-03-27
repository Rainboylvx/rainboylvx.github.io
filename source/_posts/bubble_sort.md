---
title: 冒泡排序
date: 2015-10-16 17:01
tags: 排序,基础
categories: 算法
---

## 冒泡排序的讲解

**一句话算法：第一层循环len-1次,第二层第i次循环len-i次**

> 我们把 **i** 作为我们层循环的次数:那么 i:1-->len-1(len 数组的长度,我们的下标从1开始)
    j代表我们的第二层循环,那么我们想一下:
    第i次循环 循环停止的下标
    
    i:1       len-1
    i:2       len-2
    .......
    i:i       len-i
    


## bubble_sort代码

```
void bubble_sort(int a[],int len){
    int i=len,j;
    for(i=1;i<=len-1;i++)
        for(j=1;j<=len-i;j++){
            if(a[j] > a[j+1]){
                tmp =a[j];
                a[j] =a[j+1]
                a[j+1]=tmp;
            }
        }
}
```

