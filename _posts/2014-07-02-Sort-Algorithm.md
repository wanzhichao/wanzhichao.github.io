---
layout: post
title: "内部排序算法分析"
description: "内部排序算法"
keywords: "排序算法"
category: 算法
tags: [插入排序,归并排序,选择排序,冒泡排序,快速排序]
---
{% include JB/setup %}

### 内部排序算法（插入排序,归并排序,选择排序,冒泡排序,快速排序）

####排序简介

排序是数据处理中经常使用的一种重要运算,在计算机及其应用系统中,花费在排序上的时间在系统运行时间中占有很大比重;并且排序本身对推动算法分析的发展也起很大作用。目前已有上百种排序方法，但尚未有一个最理想的尽如人意的方法，本章介绍常用的如下排序方法，并对它们进行分析和比较。

<!-- more -->
####排序分类

    1、插入排序（直接插入排序、折半插入排序、希尔排序）；
    2、交换排序（起泡排序、快速排序）；
    3、选择排序（直接选择排序、堆排序）；
    4、归并排序；
    5、基数排序；

####排序时间复杂度

1.插入类排序

将无序子序列中的一个或几个记录“插入”到有序序列中，从而增加记录的有序子序列的长度

2.交换类排序

通过“交换”无序序列中的记录从而得到其中关键字最小或最大的记录，并将它加入到有序子序列中，以此方法增加记录的有序子序列的长度。

3.选择类排序

从记录的无序子序列中“选择”关键字最小或最大的记录，并将它加入到有序子序列中，以此方法增加记录的有序子序列的长度。

4.归并排序

通过“归并”两个或两个以上的记录有序子序列，逐步增加记录有序序列的长度。

5.基数排序

一种非比较型整数排序算法，其原理是将整数按位数切割成不同的数字，然后按每个位数分别比较。由于整数也可以表达字符串和特定格式的浮点数，所以基数排序也不只是使用于整数。

![蔡金林的博客之时间复杂度](/assets/images/timecomplexity.png)

####一、插入排序

 直接插入排序（Insertion Sort）的算法描述是一种简单直观的排序算法。它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。插入排序在实现上，通常采用in-place排序（即只需用到O(1)的额外空间的排序），因而在从后向前扫描过程中，需要反复把已排序元素逐步向后挪位，为最新元素提供插入空间。
    
    void InsertSort(int L[],int length)  
    {  
        int i,j;//分别为有序区和无序区指针  
        for(i=1;i<length;i++)//逐步扩大有序区  
        {  
            j=i+1;  
            if(L[j]<L[i])  
            {  
             L[0]=L[j];//存储待排序元素  
             While(L[0]<L[i])//查找在有序区中的插入位置，同时移动元素  
             {  
            L[i+1]=L[i];//移动  
            i--;//查找  
             }  
            L[i+1]=L[0];//将元素插入  
        }  
       i=j-1;//还原有序区指针  
    }  
  

![插入排序](/assets/images/InsertSort.png)

####二、交换排序

 冒泡排序（Bubble Sort）是一种简单的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。

    void BubbleSort(int a[], int n)
    {
        int i,j;
        for(j=0;j<=n-1;j++)  
        {  
          for(i=0;i<=n-1-j;i++)  
          {  
            if(a[i]>a[i+1])//数组元素大小按升序排列  
            {  
               temp=a[i];  
               a[i]=a[i+1];  
               a[i+1]=temp;  
            }  
          }
        }
    }  

 快速排序（Quicksort）是对冒泡排序的一种改进。它的基本思想是：运用分治法,通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另一部分小。分别对两部分排序，然后递归进行。

    int Partition(int a[], int low, int high)
    {
        int temp = a[row];
        while(low<high)
        {
          while(low<high && a[high] >= temp) high--;
          a[low] = a[high];
          while(low<high && a[low] <= temp) low++;
          a[high] = a[low];
        }
        a[low] = temp;
    }
    void Quick_sort(int a[],int low ,int high)
    {
        if(low < high)
        {
          i=Partition(a,low,high);
          Quick_sort(a,low,i-1);
          Quick_sort(a,i+1,high);
        }
    }

![快速排序](/assets/images/QuickSort.png)

####三、选择排序

 直接选择排序(Selection sort)是一种简单直观的排序算法。原理是将序列划分为无序和有序区，寻找无序区中的最小值和无序区的首元素交换，有序区扩大一个，循环最终完成全部排序。

    void SelectSort(int a[], int n)
    {
      for(i=0;i<n-1;i++)  
      {  
        k = i;  
        for(j=i+1;j<n;j++)  
        {  
            if(a[k] > a[j])  
            k = j;  
        }  
        if(k != i)  
        {  
            temp = a[i];  
            a[i] = a[k];  
            a[k] = temp;  
        }  
      }  
    }


####四、归并排序

 归并排序（Merge sort，台湾译作：合并排序）是建立在归并操作上的一种有效的排序算法。该算法是采用递归和分治法（Divide and Conquer）的一个非常典型的应用。

    void merge(int *a,int start,int mid,int end)  
    {  
        if(start>mid || mid >end ) return;  
        int i=start,j=mid+1,k=0;  
        int *L=(int *)malloc((end-start+1)*sizeof(int));  
        while(i<=mid && j<=end)  
        {  
          if(a[i]<a[j])  
            {  
                L[k++]=a[i++];  
            }  
            else  
            {  
                L[k++]=a[j++];  
            }  
        }    
        while(i<=mid)  
              L[k++]=a[i++];  
          while(j<=end)  
              L[k++]=a[j++];  
        for(i=start,j=0;i<=end;i++,j++)  
        {  
              a[i]=L[j];  
        }  
        free(L);  
    }  
    void mergeSort(int *a, int start,int end)  
    {  
      if(start<end)  
      {  
          int mid=(start+end)/2;  
          mergeSort(a,start,mid);  
          mergeSort(a,mid+1,end);  
          merge(a,start,mid,end);  
      }  
    }  

![归并排序](/assets/images/MergeSort.png)

####五、基数排序

基数排序(Radix sort)是一种非比较型整数排序算法，其原理是将整数按位数切割成不同的数字，然后按每个位数分别比较。由于整数也可以表达字符串（比如名字或日期）和特定格式的浮点数，所以基数排序也不是只能使用于整数。


####快速排序与归并排序，冒泡排序与选择排序关系

![排序之间关系](/assets/images/relation.png)

