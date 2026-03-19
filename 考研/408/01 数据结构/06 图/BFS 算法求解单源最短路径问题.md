---
名称: BFS 算法求解单源最短路径问题
章: 第六章 图
节: "[[第三节 图的遍历]]"
tags:
  - 知识点
index: 3
---
若图 G = (V, E) 为非带权图，定义从顶点 u 到顶点 v 的 **最短路径 d(u, v)** 为所有从 u 到 v 的路径中所含边数的最小值；若从 u 到 v 不可达，则 d(u,v)=∞

利用 BFS 可求解 **非带权图的单源最短路径问题**，这是因为广度优先搜索总是按照距离由近到远的顺序遍历图中顶点，从而保证每个顶点首次被访问时，所经过的路径即为最短路径

##### BFS 求解单源最短路径的实现

```c
void BFS_MIN_Distance(Graph G,int u){
    //d[i] 表示从 u 到 i 的最短路径
    for(i=0;i<G.vexnum;++i)
        d[i]=∞;                   //初始化路径长度
    visited[u]=TRUE; d[u]=0;
    EnQueue(Q,u);
    while(!QueueEmpty(Q)){       //BFS 算法主过程
        DeQueue(Q,u);             //队头元素 u 出队
        for(w=FirstNeighbor(G,u);w>=0;w=NextNeighbor(G,u,w))
            if(!visited[w]){      //w 为 u 的尚未访问的邻接顶点
                visited[w]=TRUE;  //设已访问标记
                d[w]=d[u]+1;      //路径长度加 1
                EnQueue(Q,w);     //顶点 w 入队
            }
    }
}
```
