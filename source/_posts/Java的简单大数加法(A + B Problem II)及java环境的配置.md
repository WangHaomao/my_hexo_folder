---
title: Java的简单大数加法(A + B Problem II)及java环境的配置
date: 2015-08-05 11:34:22
tags: "ACM&OJ"
---

最近讲到高精度专题了，于是果断想到了用Java，虽然以前没接触过Java，还是被它的方便性吸引了，下个JDK和eclipsean安装完再配置一下Java环境就可以了，下面稍微提一下环境配置吧（我搞了好久，不过网上好多教程）

桌面右键我的电脑（计算机）-->属性-->高级系统设置-->（高级中的）环境变量:

在系统变量里面最后要出现这三个变量（有就改一下变量值，没有就新建）
```
变量名
变量值
JAVA_HOME 
Java的安装路径，要求安装JDK的时候的路径（我的是G :\ JAVA \JDK）

PATH
%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;

CLASSPATH
.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;
 （直接复制就好了）
```
接下来看一道题
http://acm.hdu.edu.cn/showproblem.php?pid=1002
```Java
import java.io.*;    
import java.math.*;    
import java.util.*;    
import java.text.*;   
  
public class Main{  
    public static void main(String[] args)  
    {  
        Scanner cin = new Scanner (new BufferedInputStream(System.in));  
        int times=1;  
        int T;  
        T=cin.nextInt();  
        while(T!=0)  
        {  
            BigInteger a;  
            BigInteger b;  
            a=cin.nextBigInteger();  
            b=cin.nextBigInteger();  
            System.out.println("Case "+ (times++)+":");  
            System.out.println(a + " + " + b + " = " + a.add(b));  
            if(T!=1) System.out.println("");  
            T--;  
        }  
          
    }  
}  
```

```Java
public class Main //这里各个OJ默认的不同,此处只能用Main  
```