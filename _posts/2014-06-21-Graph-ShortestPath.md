---
layout: post
title: "图论之最短路算法"
description: "图论之最短路算法"
keywords: "Dijkstra算法、Bellman-Ford算法、SPFA算法"
category: 算法
tags: [图论]
---
{% include JB/setup %}

### Dijkstra算法

Dijkstra算法解决了有向加权图的最短路径问题，该算法的条件是该图所有边的权值非负，对于每条边(u,v) E，w(u,v)>=0。
Dijkstra算法中设置了一结点集合S，从源结点s到集合S中结点的最终最短路径的权均已确定，即对所有结点v S，有d[v]= (s,v)。
算法反复挑选出其最短路径估计为最小的结点u V-S，把u插入集合S中，并对离开u的所有边进行松弛。
在下列算法实现中设置了优先队列Q，该队列包含所有属于V-S的结点，且队列中各结点都有相应的d值。
Dijkstra算法如图所示对边进行松弛操作，最左结点为源结点，每个结点内为其最短路径估计。

<!-- more -->      

![邻接表](/assets/images/Djs.png)

因为Dijkstra算法总是在集合V-S中选择“最轻”或“最近”的结点插入集合S中，因此我们说它使用了贪心策略。需要指出的是，贪心策略并非总能获得全局意义上的最理想结果。但Dijkstra算法确实计算出了最短路径。

### Bellman—Ford算法

Bellman-Ford算法能在更一般的情况下解决单源点最短路径问题，在该算法下边的权可以为负。正如Dijkstra算法一样，Bellman-Ford算法运用了松弛技术，对每一结点vV，逐步减小从源s到v的最短路径的估计值d[v]直至其达到实际最短路径的权(s,v)，如果图中存在负权回路，算法将会报告最短路不存在。

源结点为z。每个结点内为该结点的d值，阴影覆盖的边说明了值。在该实例中，Bellman-Ford算法返回TRUE。在进行了通常的初始化后，算法对图的边执行|V|-1次操作。每次均为第2-4行For循环的一次迭代，在迭代过程中对图的每条边松弛一次，图(b)-(c)说明了全部四次操作的每一次后算法的状态,在进行完|V|-1次操作后,算法5-8行检查是否存在负权的回路并返回正确的布尔值。

Bellman-Ford算法的运行时间为O(VE)。因为第1行的初始化占用时间为O(V)，第2-4行对边进行的|V|-1次操作的每一次运行时间为O(E)，第5-7行的For循环的运行时间为O(E)。

![邻接表](/assets/images/Ford.png)

Bellman-Ford算法的思想基于以下事实：“两点间如果有最短路，那么每个结点最多经过一次。也就是说，这条路不超过n-1条边。”（如果一个结点经过了两次，那么我们走了一个圈。如果这个圈的权为正，显然不划算；如果是负圈，那么最短路不存在；如果是零圈，去掉不影响最优值）

### SPFA算法

求单源最短路的SPFA算法的全称是：Shortest Path Faster Algorithm。
从名字我们就可以看出，这种算法在效率上一定有过人之处。

很多时候，给定的图存在负权边，这时类似Dijkstra等算法便没有了用武之地，而Bellman-Ford算法的复杂度又过高，SPFA算法便派上用场了。

简洁起见，我们约定有向加权图G不存在负权回路，即最短路径一定存在。当然，我们可以在执行该算法前做一次拓扑排序，以判断是否存在负权回路。

和上文一样，我们用数组d记录每个结点的最短路径估计值，而且用邻接表来存储图G。我们采取的方法是动态逼近法：设立一个先进先出的队列用来保存待优化的结点，优化时每次取出队首结点u，并且用u点当前的最短路径估计值对离开u点所指向的结点v进行松弛操作，如果v点的最短路径估计值有所调整，且v点不在当前的队列中，就将v点放入队尾。这样不断从队
列为空。

### 案例分析

畅通工程续 SPFA||dijkstra||floyd

题目链接：http://acm.hdu.edu.cn/showproblem.php?pid=1874

Dijkstra算法实现c++代码

    #include <iostream>
    #include<memory.h>
    using namespace std;
    #define MAX 200
    #define INF 1000000
    int map[MAX][MAX];
    int dist[MAX];
    int visit[MAX];
    int n,m;
    void dfs(int s)
    {
       int vexnum=s;
        int i,j;
        memset(visit,0,sizeof(visit));
        dist[vexnum]=0;
        visit[vexnum]=1;
        for(i=0;i<n;i++)
        {
           for(j=0;j<n;j++)
           {
              if(!visit[j]&&dist[j]>(dist[vexnum]+map[vexnum][j]))
              {
                 dist[j]=dist[vexnum]+map[vexnum][j];
              }
         }
         int min=INF;
         for(j=0;j<n;j++)
         {
             if(!visit[j]&&dist[j]<min)
             {
                 min=dist[vexnum=j];
             }
         }
         visit[vexnum]=1;
       }
    }
    int main()
    {
      int i,j;
      int a,b,x;
      int s,t;
      while(cin>>n>>m)
      {
        for(i=0;i<n;i++)
        {
            dist[i]=INF;
            for(int j=0;j<n;j++)
                map[i][j]=INF;
        }
        for(i=0;i<m;i++)
        {
            cin>>a>>b>>x;
            if(map[a][b]>x)
            {
                map[a][b]=map[b][a]=x;
            }
        }
        cin>>s>>t;
        dfs(s);
        if(dist[t]!=INF)
        {
            cout<<dist[t]<<endl;
        }
        else
        {
            cout<<"-1"<<endl;
        }
     }
    return 0;
    }

 
### 案例分析：Wormholes

题目链接http://poj.org/problem?id=3259

SPFA算法+邻接表实现版本：

    #include<iostream>
    #include<stdio.h>
    using namespace std;
    struct node
    {
      int u,v,w;
    }edge[6000];
    int dis[505];
    int n,m,w,index;
    const int inf=0x7ffffff;
    void add(int u,int v,int c)
    {
        index++;
        edge[index].u=u;
        edge[index].v=v;
        edge[index].w=c;
    }
    bool bellman()
    {
      int u,v,w,i,j,flag;
      for(i=1;i<=n;i++)
      {
          dis[i]=inf;
      }
      dis[1]=0;flag=0;
      for(i=1;i<=n;i++)
      {
        for(j=1;j<=index;j++)
        {
            if(dis[edge[j].v]>dis[edge[j].u]+edge[j].w)
            {
              dis[edge[j].v]=dis[edge[j].u]+edge[j].w;
            }
       }
      }
      for(i=1;i<=index;i++)
          if(dis[edge[i].v]>dis[edge[i].u]+edge[i].w) return true;
      return false;
      }
    int main()
    {
        int i,j,t,u,v,c;
        scanf("%d",&t);
        while(t--)
        {
           scanf("%d%d%d",&n,&m,&w);
          index=0;
          for(i=0;i<m;i++)
          {
            scanf("%d%d%d",&u,&v,&c);
            add(u,v,c);
            add(v,u,c);
          }
          for(i=0;i<w;i++)
          {
            scanf("%d%d%d",&u,&v,&c);
            add(u,v,-1*c);
          }
          if(bellman()) printf("YES\n");
              else printf("NO\n");
          }
          return 0;
    }

### SPFA算法+队列实现版本：

    #include<cstdio>
    #include<string>
    #include<queue>
    using namespace std;
    const int INF=9999999;
    const int MAXN=520;
    const int MAXM=5200;
    struct edge
    {
      int to;
      int val;
      int next;
    }e[MAXM];
    int len,head[MAXN];
    int dis[MAXN];
    int n,m,w;
    bool SPFA()
    {
        for(int i=1;i<=n;i++)
        dis[i]=INF;
        bool vis[MAXN]={0};
        int cnt[MAXN]={0};
        int cur=1;
        queue<int> q;
        q.push(cur);
        vis[cur]=true;
        cnt[cur]=1;
        dis[cur]=0;
        while(!q.empty())  
        {
           cur=q.front();
            q.pop();
            vis[cur]=false;
            for(int i=head[cur]  ;i!=-1; i=e[i].next)
            {
                int id=e[i].to;
                if( dis[cur] + e[i].val < dis[ id ] )
                {
                  dis[ id ] = dis[cur] + e[ i ].val;
                  if(!vis[id])
                  {
                    cnt[id]++;
                    vis[id]=true;
                    q.push(id);
                    if(cnt[cur]>n)
                    return true;
                  }
                }
            }
        }
        return false;
    }
    void add(int from,int to,int val)
    {
      e[len].to=to;
      e[len].val=val;
      e[len].next= head[from];
      head[from]=len++;
    }
    int main()
    {
      int T;
      scanf("%d",&T);
      while(T--)
      {
        memset(head,-1,sizeof(head));
        len=0;
        scanf("%d%d%d",&n,&m,&w);
        for(int i=0;i<m;i++)
        {
          int from,to,val;
          scanf("%d%d%d",&from,&to,&val);
          add(from,to,val);
          add(to,from,val); //双向的
        }
        for(int i=0;i<w;i++)
        {
          int from,to,val;
          scanf("%d%d%d",&from,&to,&val);
          add(from,to,-val);
        }
        if( SPFA())
          puts("YES");
        else
          puts("NO");
      }
      return 0;
    }



