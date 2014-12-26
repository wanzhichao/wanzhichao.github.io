---
layout: post
title: "图的遍历算法———BFS和DFS算法"
description: "图的遍历算法"
keywords: "深度优先遍历和广度优先遍历"
category: 算法
tags: [BFS,DFS]
---
{% include JB/setup %}

### 深度优先遍历和广度优先遍历
从图的某顶点出发，访问图中所有顶点，并且每个顶点仅访问一次。图中可能有回路，遍历可能沿回路又回到已遍历过的结点。
为避免同一顶点被多次访问，必须为每个被访问的顶点作一标志。为此引入一辅助数组,记录每个顶点是否被访问过。

<!-- more -->            

####1.使用邻接表来储存图的节点关系

![邻接表](/assets/images/Adjlist.png)

####2. 深度优先遍历
  
  思路:从图的某一顶点V0出发，访问此顶点；然后依次从V0的未被访问的邻接点出发，深度优先遍历图，直至图中所有和V0相通的顶点都被访问到；

  若此时图中尚有顶点未被访问，则另选图中一个未被访问的顶点作起点，重复上述过程，直至图中所有顶点都被访问为止

![邻接表](/assets/images/Dft.png)
    
####3.广度优先遍历

  思路：从图中的某个顶点V0出发，并在访问此顶点之后依次访问V0的所有未被访问过的邻接点，之后按这些顶点被访问的先后次序依次访问它们的邻接点，直至图中所有和V0有路径相通的顶点都被访问到。

  若此时图中尚有顶点未被访问，则另选图中一个未曾被访问的顶点作起始点，重复上述过程，直至图中所有顶点都被访问到为止

![邻接表](/assets/images/Bft.png)

####4.BFS和DFS算法实现c++版

    #include<iostream>
    #include<queue>
    #include<memory.h>
    using namespace std;
    const int n=9;
    int visit[100];//访问标志
    void BFS(int a[][n] , int n) //宽度优先遍历
    {
      queue<int> g;
      int i;
      memset(visit,0,sizeof(visit));
      g.push(0);
      cout<<0<<" ";
      visit[0]=1;
      while(!g.empty())
      {
         int vexnum=g.front();
        g.pop();
        for(i=0;i<n;i++)
        {
           if(a[vexnum][i]!=0&&!visit[i])
           {
               g.push(i);
               visit[i]=1;//设置进栈标志
               cout<<i<<" ";
           }
        }
      }
    }
    void DFS(int a[][n], int v)
    {
        visit[v]=1;
        cout<<v<<" ";
        for(int i=0;i<n;i++)
        {
            if(a[v][i]!=0&&!visit[i])
            {
                DFS(a, i);
            }
        }
    }
    int main(int argc, char* argv[])
    {
        //邻接矩阵9*9，节点0~8
        int a[n][n]={
        1,1,0,0,1,1,1,1,0,
        1,1,0,1,0,0,1,1,1,
        0,0,1,1,0,1,1,0,1,
        0,1,1,1,1,1,1,0,0,
        1,0,0,1,1,0,1,1,0,
        1,0,1,1,0,1,1,1,1,
        1,1,1,1,1,1,1,0,1,
        1,1,0,0,1,1,0,1,1,
        0,1,1,0,0,1,1,1,1
    };
    for(int i=0;i<n;i++)
    {
        cout<<i<<":";
        for(int j=0;j<n;j++)
        {
            if(a[i][j]!=0&&i!=j)
            {
                cout<<j;
            }
        }
        cout<<endl;
    }
    cout<<"广度优先遍历："<<endl;
    BFS(a,n);
    cout<<endl;
    memset(visit,0,sizeof(visit));
    cout<<"深度优先遍历："<<endl;
    DFS(a,0);
    return 0;
    }