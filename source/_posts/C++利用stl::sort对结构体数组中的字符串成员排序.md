---
title: C++利用stl::sort对结构体数组中的字符串成员排序
date: 2015-07-26 12:54:32
tags: "Tools"
---

之前发过的帖中，有讲到过对结构体字符串进行排序的，除了手写之外，便想到用C/C++中的qsort来对结构体数组中的字符串进行排序。但是推广到sort中时，想了好久也没想明白，看看网上这样的帖也比较少，其实还是很好理解的，主要是std::sort的cmp函数要求是bool的返回值，随意抓住这点就可以。

问题大概是这样：

```C++
#include <iostream>  
#include <cstdio>  
#define MAXN 50  
using namespace std;  
  
struct tmpToStruct{  
    char ArraryString[MAXN];  
};  
  
tmpToStruct step[5+1];  
  
int main()  
{  
    for(int i=0;i<5;i++)  
        scanf("%s",step[i].ArraryString);  
    return 0;  
}  
```

对step里的ArraryString按照字符串的字典序进行排序，下面给出sort排序的方法：
```C++
#include <iostream>  
#include <cstdio>  
#include <cstring>  
#include <algorithm>  
#define MAXN 50  
using namespace std;  
  
struct tmpToStruct{  
    char arrStr[MAXN];  
};  
  
tmpToStruct step[5+1];  
  
bool upCmp(const tmpToStruct &tmp1,const tmpToStruct &tmp2){  
         return strcmp(tmp1.arrStr,tmp2.arrStr)<0 ? true : false;  
}  
  
bool downCmp(const tmpToStruct &tmp1,const tmpToStruct &tmp2){  
         return strcmp(tmp1.arrStr,tmp2.arrStr)>0 ? true : false;  
}  
int main()  
{  
    puts("before the sort:");  
    for(int i=0;i<5;i++)  
        scanf("%s",step[i].arrStr);  
    cout << endl;  
  
    sort(step,step+5,upCmp);  
    puts("after the up sort:");  
    for(int i=0;i<5;i++)  
        puts(step[i].arrStr);  
    cout << endl;  
  
    puts("after the down sort:");  
    sort(step,step+5,downCmp);  
    for(int i=0;i<5;i++)  
        puts(step[i].arrStr);  
    cout << endl;  
    return 0;  
}  
```
对比下qsort的排序，还是很简短的。