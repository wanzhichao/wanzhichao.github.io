---
layout: post
title: "最小生成树"
descript: "生成树"
keywords: "图论"
category: 算法
tages: [Prim,kruskal]
---
{% include JB/setup %}

###图论之最小生成树(Prim算法和Kruskal算法)

所有顶点均由边连接在一起，但不存在回路的图叫生成树,在一给定的无向图G = (V, E) 中，(u, v) 代表连接顶点 u 与顶点 v 的边（即），而 w(u, v) 代表此边的权重，若存在 T 为 E 的子集（即）且为无循环图，使得的 w(T) 最小，则此 T 为 G 的最小生成树。最小生成树其实是最小权重生成树的简称

<!-- more -->

####生成树的遍历方式

    深度优先生成树
    广度优先生成树

![生成树的遍历方式](/assets/images/tree.png)

####生成树的特点
    
    生成树的顶点个数与图的顶点个数相同
    生成树是图的极小连通子图
    一个有n个顶点的连通图的生成树有n-1条边
    生成树中任意两个顶点间的路径是唯一的
    在生成树中再加一条边必然形成回路

####构造最小生成树的方法

#####prim算法基本思想

取图中任意一个顶点 v 作为生成树的根，之后往生成树上添加新的顶点 w。在添加的顶点 w 和已经在生成树上的顶点v 之间必定存在一条边，并且该边的权值在所有连通顶点 v 和 w 之间的边中取值最小。之后继续往生成树上添加顶点，直至生成树上含有 n-1 个顶点为止。

一般情况下所添加的顶点应满足下列条件:

    在生成树的构造过程中，图中 n 个顶点分属两个集合
    已落在生成树上的顶点集 U 和尚未落在生成树上的顶点集V-U ，则应在所有连通U中顶点和V-U中顶点的边中选取权值最小的边。

#####prim方法实现过程原理图

![prim算法原理图](/assets/images/prim.png)

#####prim算法描述

    Prim(Mgraph G, VexsType v0)
    { 
    // v0－初始出发点，T－生成对中边的集合
    // U－生成树中顶点的集合
    T=Φ
    U={v0}
    while(U<>V)
    {
        求u,v,使得 c(u,v)最小，u €U, v €V-U
        T=TU{(u,v)}
        U=UU{v}
    }
    ｝

#####kruskal算法基本思想

具体做法：

先构造一个只含 n 个顶点的子图 SG，然后从权值最小的边开始，若它的添加不使SG 中产生回路，则在 SG 上加上这条边，如此重复，直至加上 n-1 条边为止考虑问题的出发点：

为使生成树上边的权值之和达到最小，则应使生成树中每一条边的权值尽可能地小

#####kruskal算法实现过程原理图

![kruskal算法原理图](/assets/images/kruskal.png)

####prim和Kruskal算法具体实现c++版(官方)

    #include<iostream>
    #include<cstdlib>
    #include<cstdio>
    #define MAX_VERTEX_NUM 20 //保存节点个数
    #define OK 1
    #define ERROR 0
    #define MAX 1000
    using namespace std;
    typedef struct Arcell//保存邻接矩阵
    {
    double adj;//边的权值
    }Arcell,AdjMatrix[MAX_VERTEX_NUM][MAX_VERTEX_NUM];
    typedef struct MGraph
    {
    char vexs[MAX_VERTEX_NUM];//节点数组
    AdjMatrix arcs;//邻接矩阵
    int vexnum,arcnum;//图的节点个数和边或弧数
    }MGraph;
    typedef struct Pnode//用于普利姆prim算法
    {
    char adjvex;//节点
    double lowcost;//权值
    }Pnode,Closedge[MAX_VERTEX_NUM];//记录顶点集U到V-U的代价最小的边的辅助数组定义
    typedef struct Knode//用于克鲁斯卡尔算法中存储一条边及其对应的2个节点
    {
    char ch1;//节点1
    char ch2;//节点2
    double value;//权值
    }Knode,Dgevalue[MAX_VERTEX_NUM];

    //-------------------------------------------------------------------------------
    int CreateUDG(MGraph &G,Dgevalue &dgevalue);
    int LocateVex(MGraph G,char ch);
    int Minimum(MGraph G,Closedge closedge);
    void MiniSpanTree_PRIM(MGraph G,char u);
    void Sortdge(Dgevalue &dgevalue,MGraph G);

    //-------------------------------------------------------------------------------
    int CreateUDG(MGraph &G,Dgevalue &dgevalue)//构造无向加权图的邻接矩阵
    {
    int i,j,k;
    cout<<"请输入图中节点个数和边/弧的条数："; cin>>G.vexnum>>G.arcnum;
    cout<>G.vexs[i];
    for(i=0;i<G.vexnum;++i)//初始化数组
    {
    for(j=0;j>dgevalue[k].ch1>>dgevalue[k].ch2>>dgevalue[k].value;
    i=LocateVex(G,dgevalue[k].ch1);
    j=LocateVex(G,dgevalue[k].ch2);
    G.arcs[i][j].adj=dgevalue[k].value;
    G.arcs[j][i].adj=G.arcs[i][j].adj;
    }
    return OK;
    }
    int LocateVex(MGraph G,char ch)//确定节点ch在图G.vexs中的位置
    {
    int a;
    for(int i=0;idgevalue[j].value)
    {
    temp=dgevalue[i].value;
    dgevalue[i].value=dgevalue[j].value;
    dgevalue[j].value=temp;
    ch1=dgevalue[i].ch1;
    dgevalue[i].ch1=dgevalue[j].ch1;
    dgevalue[j].ch1=ch1;
    ch2=dgevalue[i].ch2;
    dgevalue[i].ch2=dgevalue[j].ch2;
    dgevalue[j].ch2=ch2;
    }
    }
    }
    }
    int main()
    {
    int i,j;
    MGraph G;
    char u;
    Dgevalue dgevalue;
    CreateUDG(G,dgevalue);
    cout<<"图的邻接矩阵为："<<endl;
    for(i=0;i<G.vexnum;i++)
    {
    for(j=0;j<&G.vexnum;j++)
    cout<<G.arcs[i][j].adj<<"   ";
    cout<<endl;
    }
    cout<<"=============普利姆Prim算法===============\n";
    cout<<"请输入起始点：";
    cin>>u;
    cout<<"构成最小代价生成树的边集为：\n";
    MiniSpanTree_PRIM(G,u);
    cout<<"============克鲁斯科尔Kruskal算法=============\n";
    cout<<"构成最小代价生成树的边集为：\n";
    MiniSpanTree_KRSL(G,dgevalue);
    return 0;
    }

####案例分析:：Constructing Roads

题目链接：http://poj.org/problem?id=2421

算法思想：运用prim方法构造最小生成树

    #include<iostream>
    #include<cstdio>
    #include<cstring>
    using namespace std;
    #define INF 0xffffff
    int arics[105][105];
    int visit[105],dis[105],sum,n;
    void prim()
    {
    	memset(visit,0,sizeof(visit));
    	int i,j;
    	for(i=1;i<=n;i++)
    	{
    	    dis[i]=arics[1][i];
    	}
        visit[1]=1;
    	for(i=1;i<=n;i++)
    	{
    	    int temp=INF;
    	    int k;//记录最小边的顶点序号
    	    for(j=1;j<=n;j++)
    	    {
    	        if((!visit[j])&&(temp>dis[j]))
    	        {
                    temp=dis[j];
                    k=j;
    	        }
    	    }
    	    if(temp==INF)
    	    {
    	        break;
    	    }
    	    visit[k]=1;//置为已访问标志
    	    sum+=dis[k];
    	    for(j=1;j<=n;j++)
    	    {
    	        if(!visit[j])
    	        {
    	            if(dis[j]>arics[k][j])
    	            {
    	                dis[j]=arics[k][j];//更新值
    	            }
    	        }
    	    }
    	}

    }
    int main()
    {
        int i,j,a,b,q;
        cin>>n;//输入节点个数
        for(i=1;i<=n;i++) //输入带权邻接矩阵
        {
            for(j=1;j<=n;j++)
            {
                cin>>arics[i][j];
            }
        }
        cin>>q;//已经建了公路的个数
        for(i=1;i<=q;i++)
        {
             cin>>a>>b;//输入公路的两端（节点）
             arics[a][b]=arics[b][a]=0;
        }
        sum=0;
        prim();
        cout<<sum<<endl;
        return 0;
    }
