---
title: ExtendGcd,即扩展欧几里得算法的C++模板化解释
date: 2015-07-21 20:29:30
tags: "ACM&OJ"
---

刚刚接触感觉很高大上的“扩展欧几里得算法“，很郁闷，研究了很久。现在感觉能够套模板了，当然这样是远远不够的，不过先写篇博客记录一下最近的动态。帮助自己记忆，也可以帮助大家理解下这个数学算法，当然欢迎各位的斧正和指点，我将不断努力！

首先，明确我们要求ax+by=c中x,y的整数解（至于没有解的情况下边会讨论）

大家应该看到过ax+by=Gcd(a,b)的式子，现在我也不明白这是什么，以下是我大概能够死记硬背的（大家能学会的还是去学学原理）。

先求gcd(a,b),程序如下:


```C
typedef long long LL;  
using namespace std;  
  
LL gcd(LL a,LL b){  
    while(a%b){  
        LL temp=b;  
        b=a%b;  
        a=temp;  
    }  
    return b;  
}  
```
LL d=gcd(a,b);
后，a/=d,b/=d,c/=d;这里有当c%d!=0是，ax+by=c不存在整数解（我也不知道为什么，真的模板化了)

于是原式变成a'x+b'y=c'。据说那个扩展欧几里得求的是a'x+b'y=1的解，给出extendGcd(a,b)的模板


```C
void exGcd(LL a,LL b,LL &x,LL &y){
    if(b==0){
        x=1;y=0;  
        return ;  
    } 
    exGcd(b,a%b,x,y); 
        LL temp;  
        temp=y;  
        y=x-a/b*y;  
        x=temp;  
}
```
这里不难得到修改后的x,y为方程a'x+b'y=1的解，那么c'x0,c'y0就是a'x+b'y=c'的一组特解了，根据参数方程的性质，我们引入t（整数）来写出x,y通解的参数方程x=c'x0-b't，y=c'y0+a't。
通常题目要求我们求问题的最小解，所以当x->0时，我们求出的t=c'x0/b(这里是有误差的，因为在C语言中的除不一定，事实上，我们可以判断下x<0时，可以让t=t+1。

下面来看一道典型的模板题吧。

poj 1061 


青蛙的约会
Time Limit: 1000MS	 	Memory Limit: 10000K
Total Submissions: 96453	 	Accepted: 18021
Description

两只青蛙在网上相识了，它们聊得很开心，于是觉得很有必要见一面。它们很高兴地发现它们住在同一条纬度线上，于是它们约定各自朝西跳，直到碰面为止。可是它们出发之前忘记了一件很重要的事情，既没有问清楚对方的特征，也没有约定见面的具体位置。不过青蛙们都是很乐观的，它们觉得只要一直朝着某个方向跳下去，总能碰到对方的。但是除非这两只青蛙在同一时间跳到同一点上，不然是永远都不可能碰面的。为了帮助这两只乐观的青蛙，你被要求写一个程序来判断这两只青蛙是否能够碰面，会在什么时候碰面。 
我们把这两只青蛙分别叫做青蛙A和青蛙B，并且规定纬度线上东经0度处为原点，由东往西为正方向，单位长度1米，这样我们就得到了一条首尾相接的数轴。设青蛙A的出发点坐标是x，青蛙B的出发点坐标是y。青蛙A一次能跳m米，青蛙B一次能跳n米，两只青蛙跳一次所花费的时间相同。纬度线总长L米。现在要你求出它们跳了几次以后才会碰面。 
Input

输入只包括一行5个整数x，y，m，n，L，其中x≠y < 2000000000，0 < m、n < 2000000000，0 < L < 2100000000。
Output

输出碰面所需要的跳跃次数，如果永远不可能碰面则输出一行"Impossible"
Sample Input
1 2 3 4 5
Sample Output
4

```C
#include <iostream>  
#include <cstdio>  
  
typedef long long LL;  
using namespace std;  
  
LL gcd(LL a,LL b){  
    while(a%b){  
        LL temp=b;  
        b=a%b;  
        a=temp;  
    }  
    return b;  
}  
  
void exGcd(LL a,LL b,LL &x,LL &y){  
    if(b==0){  
        x=1;y=0;  
        return ;  
    }  
    exGcd(b,a%b,x,y);  
        LL temp;  
        temp=y;  
        y=x-a/b*y;  
        x=temp;  
}  
int main()  
{  
    LL x,y,m,n,l,ans,key,t;  
    while(~scanf("%I64d%I64d%I64d%I64d%I64d",&x,&y,&m,&n,&l)){  
        /* (n-m)*ans+k*l=x-y; 
         * n-m=a,ans=x,k=y,l=b,x-y=c; 
         *   a*x+b*y=c; 
         */  
        LL a=n-m,b=l,c=x-y;  
        LL d=gcd(a,b);  
        //cout <<  d  <<endl;  
        if(c%d) {printf("Impossible\n");continue;}  
  
        a/=d;b/=d;c/=d;  
        exGcd(a,b,ans,key);  
  
        t=c*ans/b;  
        ans=c*ans-t*b;  
        if(ans<0)  
            ans+=b;  
        printf("%I64d\n",ans);  
    }  
    return 0;  
}  
```