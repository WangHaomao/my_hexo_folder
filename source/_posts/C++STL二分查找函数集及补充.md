---
title: C++STL二分查找函数集及补充
date: 2015-08-04 20:53:39
tags: "Tools"
---

C++中还是有许多我未了解或者不常用的函数，比如今天写下的一些二分函数，还是挺有用的，不仅仅可以减少代码量，而且能够增加时间的利用率。（目前我只学了简单的数组查找，有待补充）

大概有以下几个STL中的二分查找函数：

```C++
binary_search(a,a+n,key)    //返回是否存在值bool型的  
lower_bound(a,a+n,key)      //下面两个都是指针型的  
upper_bound(a,a+n,key)  
/*升序排列的容器： 
lower_bound( const key_type &key ): 返回一个迭代器，指向键值>= key的第一个元素。 
upper_bound( const key_type &key ): 返回一个迭代器，指向键值>key的第一个元素。 
//降序排列的容器： 
lower_bound( const key_type &key ): 返回一个迭代器，指向键值<= key的第一个元素。 
upper_bound( const key_type &key ):返回一个迭代器，指向键值<key的第一个元素。 
*/  
```

以下介绍一下手工写的二分函数，也可以改成系STL的函数：
一、判断是否存在key值：

```C++
bool search(int A[], int n, int target)  
{  
    int low = 0, high = n-1;  
    while(low <= high)  
    {  
        int mid = low+((high-low)>>1);  
        if(A[mid] == target)  
            return true;  
        else if(A[mid] < target)  
            low = mid+1;  
        else  
            high = mid-1;  
    }  
    return false;  
}  
```
二、存在相同key值时查找最先出现的位置：

```C++
//此函数返回的是第几个元素，非数组下标  
int lower_search(int A[], int n, int target)  
{  
    int low = 0, high = n-1;  
    while(low <= high)  
    {  
        int mid = low+((high-low)>>1);  
        if(A[mid] == target)  
        {  
            if(mid > 0 && A[mid-1] == target)  
                high = mid-1;  
            else  
                return mid;  
        }  
        else if(A[mid] < target)  
            low = mid+1;  
        else  
            high = mid-1;  
    }  
    return -(low+1);  
}  
```
三、存在相同元素的时候，返回最后出现的位置：
```C++
[cpp] view plain copy
//此函数返回的是第几个元素，非数组下标  
int upper_search(int A[], int n, int target)  
{  
    int low = 0, high = n-1;  
    while(low <= high)  
    {  
        int mid = low+((high-low)>>1);  
        if(A[mid] == target)  
        {  
            if(mid < n-1 && A[mid+1] == target)  
                low = mid+1;  
            else  
                return mid;  
        }  
        else if(A[mid] < target)  
            low = mid+1;  
        else  
            high = mid-1;  
    }  
    return -(low+1);  
}  
```
三、存在多个key值时，返回位置差：
```C++
int equal_search(int A[], int n, int target){  
    int tmp = upper_search(A, n, target)-lower_search(A, n, target);  
    return tmp;  
}  
```