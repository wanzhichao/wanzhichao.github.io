---
layout: post
title: "高精度算法"
description: "高精度算法"
keywords: "高精度算法"
category: 算法
tags: [高精度]
---
{% include JB/setup %}

### 高精度算法（包含精度计算乘法、精度计算加法等）

高数精度算法，属于处理大数字的数学计算方法。在一般的科学计算中，会经常算到小数点后几百位或者更多，当然也可能是几千亿几百亿的大数字。一般这类数字我们统称为高精度数，高精度算法是用计算机对于超大数据的一种模拟加，减，乘，除，乘方，阶乘，开方等运算。对于非常庞大的数字无法在计算机中正常存储，于是，将这个数字拆开，拆成一位一位的，或者是四位四位的存储到一个数组中， 用一个数组去表示一个字，这样这个数字就被称谓是高精度数。高精度算法就是能处理高精度数各种运算的算法。

<!-- more -->
![蔡金林的博客之高精度算法](/assets/images/precision.png)

在acmer眼里看来，大数字之间的运算一般都会用字符串的形式来处理，例如大数求阶乘，乘法，加法等。

北电上一道关于高精度算法的算法题，题目链接[http://poj.org/problem?id=1001](http://poj.org/problem?id=1001)，用了一下午的时间开始搜集各方面code，发现大多数杂而乱，在绞尽脑汁后，自己动手写了个，终于一次性AC。
源代码实现c++版：/*注释的代码是为了记录某些值，进行排错*/
	
	#include <iostream>
	#include <string>
	#include <algorithm>
	#include <stdio.h>;
	using namespace std;
	string add(string a, string b)//参与运算的两个数字串
	{
    int i,j,k,up,x,y,z;
    string c;//存放相加结果的字符串
    i=a.length()-1;
    j=b.length()-1;
    up=0;//进位
    while(i>=0||j>=0)
    {
            if(i<0) x='0'; else x=a[i];
            if(j<0) y='0'; else y=b[j];
            z=x-'0'+y-'0';
            if(up) z+=1;//上一位加法产生的进位
            if(z>9)
            {
                up=1;
                z%=10;
            }
            else
            {
                up=0;
            }
            c.push_back(z+'0');//压入字符串
            i--;
            j--;
    }
    if(up)//判断最高位产生的进位
    {
         c.push_back('1');
    }
    reverse(c.begin(),c.end());//字符串反转
    return c;

	}
	string multiple(string a, string b)  //计算两高精度数乘积
	{
    int i, j,k;
    string result = "0";
    for(i = a.length() - 1; i >= 0; i--)
    {
        int remain = 0;
        string str = "";
        for(j = b.length() - 1; j >= 0; j--)
        {
            int tmp = (a[i] - '0') * (b[j] - '0') + remain;
            remain = tmp / 10;
            str = (char)(tmp % 10 + '0') + str;
        }
        if(remain != 0)
        {
            str = (char)(remain + '0') + str;
        }
        for(k=i;k&lt;a.length()-1;k++)
        {
            str=str+'0';
        }
       result=add(str,result);
    }
    return result;
	}
	int main()
	{
    string s1,s2;
    int i,n,pos,j=0;
    string res="1";
    string::iterator its;
    char ch1[81];
    while(scanf("%s %d",ch1,&amp;n)!=EOF)
    {
        s1=ch1;
        i=0;
        pos=0;
        for(its=s1.begin();its!=s1.end();its++)
        {
            if(*its=='.') //如果包含小数点，删除小数点，当作整数处理
            {
                pos=i;
                s1.erase(its);
                break;
            }
            i++;
        }
        /*
        for(i=0;i<s1.size();i++)
        {
            cout<<s1[i];
        }
        cout<<endl;
        */
        for(i=1;i<=n;i++)//循环作乘法
        {
            res=multiple(s1,res);
        }
        if(pos!=0)//判断是否包含小数点
        {
              j=s1.size()-pos;//取得小数点后面数的个数
              j=j*n;//输出时将要移动的小数点位数
        }
        /*
        cout<<"j:"<<j<<endl;
        cout<<"res:";
        for(i=0;i<res.size();i++)
        {
             cout<<res[i];
        }
        cout<<endl;
        */
       string::iterator it=res.begin();
       if(pos!=0)
       {
            res.insert(it+res.size()-j,'.');
            it=res.begin();
            if(res[0]=='0'&& res[1]=='.') //删除0.0001中的小树点前面的0
            {
                res.erase(it);
            }
            while(res[res.size()-1]=='0') //删除0.5000中小数点后面的0
            {
                it=res.begin()+res.size()-1;
                res.erase(it);
            }
            if(res[res.size()-1]=='.')//如果为整数，去掉小数点
            {
                it=res.begin()+res.size()-1;
                res.erase(it);
            }
       }
        for(i=0;i<res.size();i++)//直接输出
        {
            printf("%c",res[i]);
        }
        cout<<endl;
        s1.clear();
        res="1";
    }
	}