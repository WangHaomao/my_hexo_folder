---
title: 再谈字典树：HDOJ 1671 Phone List（内存释放）
date: 2015-07-24 19:42:50
tags: "ACM&OJ"
---

前两天刚发了篇博客，讲的是两道简单的字典树，今天无意间看到了这道题，我觉得有必要再来扩充一下我的字典树知识：合理分配动态内存。记得上C++课的时候，老师讲过，用new创建的指针，程序结束后最好都用delete来删除动态内存，不然会造成内存泄露。然而以前做的题虽然内存开的很大，也没在意。看了今天这道，自己也反思了下，像很多网友说的，不要为了AC而AC，虽然说的很那啥，但这确实是值得我思考的。

看一下题吧：
http://acm.hdu.edu.cn/showproblem.php?pid=1671

题意：给你一组电话，如果里面有一些短的号码（设整个号码为i）和长一点的号码的前i个数字相同，那么电话机会自动拨打那个短号码，所以你就不能往下按了。就是你不能拨打那个长号码了。如果有这种短号码，即不能拨打某个号码，输出NO，否则输出YES；
```C++
#include <iostream>  
#include <cstdio>  
#include <cstring>  
using namespace std;  
  
struct Trie{  
    Trie *next[11];  
    bool boom;  
    Trie(){  
        for(int i=0;i<11;i++)  
            next[i]=NULL;  
        boom=true;  
    }  
};  
  
bool flag=true;  
  
void buildAndSearch(char s[],Trie *root){  
    Trie *now=root;  
    int len=strlen(s);  
    for(int i=0;i<len;i++){  
        if(now->next[s[i]-'0']==NULL)  
            now->next[s[i]-'0']=new Trie;  
        else{  
            if((now->next[s[i]-'0'])->boom==false){  
                flag=false; break;  
            }  
        }  
        now=now->next[s[i]-'0'];  
    }  
    now->boom=false;  
  
    for(int i=0;i<11;i++){  
        if(now->next[i]!=NULL){  
            flag=false;return;  
        }  
    }  
}  
void freeDom(Trie *now){  
    for(int i=0;i<11;i++){  
        if(now->next[i]!=NULL) freeDom(now->next[i]);  
    }  
    delete now;  
}  
int main()  
{  
    int T;  
    scanf("%d",&T);//scanf("\n");  
    while(T--){  
        flag=true;  
        Trie *root=new Trie;  
        int N;  
        scanf("%d",&N);scanf("\n");  
        char pn[11];  
        while(N--){  
            scanf("%s",pn);  
            buildAndSearch(pn,root);  
        }  
        if(flag) printf("YES\n");  
        else printf("NO\n");  
        freeDom(root);  
    }  
    return 0;  
}  
```
注意911(先)   91125426(后) 和 91125426(先)   911(后)，输入循序不同要都考虑进去。当然，若是使用那种建树与查询分开的做法，好像是不用讨论的，具体我没试过，最后记得将每次的动态内存删除。