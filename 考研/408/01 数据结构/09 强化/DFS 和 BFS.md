---
名称: DFS 和 BFS
章: 强化
节: "[[考点分类解答二]]"
tags:
  - 知识点
index: 11
---
##### 1）DFS 思想及在不同存储结构上的实现

```c
bool visited[MVNum];
void DFSTraverse(Graph G) {
    for (v = 0; v < G.vexnum; ++v)
        visited[v] = false;
    for (v = 0; v < G.vexnum; ++v)
        if (!visited[v]) DFS(G, v);
}
```

```c
void DFS(Graph G, int v) {
    cout << v;
    visited[v] = true;
    for (w = FirstAdjVex(G, v); w >= 0; w = NextAdjVex(G, v, w)) {
        if (!visited[w])
            DFS(G, w);
    }
}
```

从一个顶点开始，先标记访问并输出它，再去检查它的邻接点；如果邻接点未被访问过，就对邻接点再次调用 DFS

重复上述操作，直到所有顶点都被标记访问

###### DFS 核心语句

```c
for (w = FirstAdjVex(G, v); w >= 0; w = NextAdjVex(G, v, w)) {
    if (!visited[w])
        DFS(G, w);
}
```

> [!note] 邻接矩阵
> 从给定的参数顶点 v 开始，首先遍历邻接矩阵中第 v 行的所有元素，如果第 w 列的值不为 0，说明顶点 v 与顶点 w 之间存在边
> 
> 若此时顶点 w 尚未被访问，则将 w 作为参数递归调用 DFS，继续以同样方式访问 w 的所有邻接点，直到所有可达顶点都被访问为止
> 
> 时间复杂度为 $O(n^2)$

> [!note] 邻接表
> 从给定的参数顶点 v 开始，先访问 v 所对应的邻接表，获取其第一个邻接点指针 p，然后沿着邻接表的边表不断向后遍历，每次取出当前边所指向的邻接点 w，如果 w 尚未被访问，就将 w 作为参数递归调用 DFS，直到所有可达顶点都被访问为止
> 
> 时间复杂度为 $O(n+e)$

##### 2）BFS 思想及在不同存储结构上的实现

```c
bool visited[MVNum];
void BFSTraverse(Graph G) {
    for (v = 0; v < G.vexnum; ++v)
        visited[v] = false;
    for (v = 0; v < G.vexnum; ++v)
        if (!visited[v]) BFS(G, v);
}
```

```c
void BFS(Graph G, int v) {
    cout << v;
    visited[v] = true;
    InitQueue(Q);
    EnQueue(Q, v);
    while (!QueueEmpty(Q)) {
        DeQueue(Q, u);
        for (w = FirstAdjVex(G, u); w >= 0; w = NextAdjVex(G, u, w))
            if (!visited[w]) {
                cout << w;
                visited[w] = true;
                EnQueue(Q, w);
            }
    }
}
```

从一个顶点开始，先标记访问并输出它，再将它加入队列

然后不断从队列中取出队头顶点，检查该顶点的所有邻接点，如果某个邻接点未被访问过，就标记访问、输出，并加入队列

重复上述过程，直到队列为空为止

> [!note] 邻接矩阵
> 从给定的参数顶点 v 开始，首先输出顶点 v，并将其标记为已访问，将顶点 v 入队
> 
> 当队列不为空时循环执行以下操作：
> 1. 从队列中取出队头顶点 u，遍历邻接矩阵第 u 行的所有元素，依次检查每个顶点 w
> 2. 如果 G.arcs[u][w] 不为 0，说明 u 与 w 之间存在边
> 3. 如果顶点 w 尚未被访问，则输出 w、标记访问，并将 w 入队，继续检查 u 行中剩下的所有列，直到扫描完整行
> 
> 时间复杂度为 $O(n^2)$

> [!note] 邻接表
> 从给定的参数顶点 v 开始，首先输出顶点 v，并将其标记为已访问，将顶点 v 入队
> 
> 当队列不为空时循环执行以下操作：
> 1. 从队列中取出队头顶点 u，访问顶点 u 所对应的邻接表，获取第一个邻接点指针 p
> 2. 沿着邻接表不断向后遍历，每次取出当前边指向的邻接点 w
> 3. 如果 w 尚未被访问，则输出 w 并标记访问，并将 w 入队
> 4. 继续遍历邻接表的下一项，直到 p 为 NULL
> 
> 时间复杂度为 $O(n+e)$

##### 3）图与树的遍历关联

- 图的 DFS 类似于树的先序遍历
- 图的 BFS 类似于树的层序遍历

##### 4）DFS 和 BFS 的应用

| DFS | BFS |
|:---:|:---:|
| 判断环路 | （权值相同的有权图 / 无权图） |
| 拓扑排序 | 单源最短路径 |
| **判断图是否连通** | **判断图是否连通** |

通过对代码进行修改，BFS 实际上也可以用于判断环路或实现拓扑排序，但 DFS 的思路更符合这两个应用场景，所以更常用

###### DFS 判断有向图是否存在环路

1. 从某个顶点出发进行 DFS
2. 在遍历过程中，如果遇到某个邻接点处于**正在访问**的状态，说明该有向图存在环路
3. 为了准确判断，通常需要为每个顶点维护三种状态：未访问、正在访问、已访问

```c
void CheckGraphCycle(Graph G) {
    for (v = 0; v < G.vexnum; v++) {
        if (status[v] == Unvisited)
            DetectCycleDFS(G, v);
    }
    if (hasCycle)
        cout << "图中存在回路";
    else
        cout << "图中无回路";
}
```

```c
bool hasCycle = false;
void DetectCycleDFS(Graph G, int v) {
    status[v] = Visiting;                  // 正在访问
    for (w = FirstAdjVex(G, v); w >= 0; w = NextAdjVex(G, v, w)) {
        if (status[w] == Visiting)         // 再次遇到正在访问的点 → 环路
            hasCycle = true;
        if (status[w] == Unvisited)
            DetectCycleDFS(G, w);
    }
    status[v] = Visited;                   // 已访问
}
```

###### DFS 判断无向图是否存在环路

1. 从某个顶点出发进行 DFS
2. 在遍历过程中，如果遇到某个邻接点处于**正在访问**的状态，并且这个正在访问的结点不是上一步访问的结点，说明该无向图存在环路
3. 为了准确判断，通常需要为每个顶点维护三种状态：未访问、正在访问、已访问

```c
void CheckGraphCycle(Graph G) {
    for (v = 0; v < G.vexnum; v++) {
        if (status[v] == Unvisited)
            DetectCycleDFS(G, v, -1);      // 初始结点没有父结点
    }
    if (hasCycle)
        cout << "图中存在回路";
    else
        cout << "图中无回路";
}
```

```c
bool hasCycle = false;
void DetectCycleDFS(Graph G, int v, int parent) {
    status[v] = Visiting;
    for (w = FirstAdjVex(G, v); w >= 0; w = NextAdjVex(G, v, w)) {
        if (status[w] == Visiting && w != parent)
            hasCycle = true;
        if (status[w] == Unvisited)
            DetectCycleDFS(G, w, v);       // 传递当前结点作为 w 的父结点
    }
    status[v] = Visited;
}
```

###### DFS 实现拓扑排序

1. 每个顶点 v 在 DFS 退出递归前都会记录当前时间戳 time，并赋值给 finishTime[v]
2. 因为后继结点的 DFS 会先于 v 完成，因此它们的 finishTime 一定比 v 大
3. 最后按照 finishTime 从大到小排序所有顶点，就得到了一个合法的拓扑序列

```c
bool visited[MVNum];
int finishTime[MVNum];
int time;
void TopoSortDFS(Graph G, int v) {
    visited[v] = true;
    for (w = FirstNeighbor(G, v); w >= 0; w = NextNeighbor(G, v, w)) {
        if (!visited[w])
            TopoSortDFS(G, w);
    }
    time = time + 1;
    finishTime[v] = time;
}
```

###### BFS 计算单源最短路径

1. 从起点出发，将其距离初始化为 0，其余所有顶点初始化为 $\infty$，并将起点入队作为搜索起点
2. 每次从队列中取出顶点 u，访问它的所有未访问邻接点 w，并将 d[w] 设为 d[u] + 1
3. 因为 BFS 一层层推进，第一个访问到的路径一定最短，最终 d[ ] 数组记录的即为起点到每个顶点的最短路径长度

```c
void BFS_DISTANCE(Graph G, int u) {
    for (i = 0; i < G.vexnum; i++)
        d[i] = ∞;
    visited[u] = TRUE;
    d[u] = 0;
    EnQueue(Q, u);
    while (!isEmpty(Q)) {
        DeQueue(Q, u);
        for (w = FirstNeighbor(G, u); w >= 0; w = NextNeighbor(G, u, w)) {
            if (!visited[w]) {
                visited[w] = TRUE;
                d[w] = d[u] + 1;
                EnQueue(Q, w);
            }
        }
    }
}
```

###### DFS / BFS 判断图是否连通

图是连通的 $\iff$ 从任意一个顶点出发可以访问到所有其他顶点

```c
bool visited[MVNum];
void DFSTraverse(Graph G) {
    for (v = 0; v < G.vexnum; ++v)
        visited[v] = false;
    DFS(G, 0);
    for (v = 0; v < G.vexnum; ++v)
        if (!visited[v])
            return false;
    return true;
}
```

```c
bool visited[MVNum];
void BFSTraverse(Graph G) {
    for (v = 0; v < G.vexnum; ++v)
        visited[v] = false;
    BFS(G, 0);
    for (v = 0; v < G.vexnum; ++v)
        if (!visited[v])
            return false;
    return true;
}
```
