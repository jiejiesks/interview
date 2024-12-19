朴素

leetcode 743

**邻接矩阵**

```c++
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 510;

int n, m;
int g[N][N];
int dist[N];
bool st[N];

int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;

    for (int i = 0; i < n - 1; i ++ )
    {
        int t = -1;
        for (int j = 1; j <= n; j ++ )
            if (!st[j] && (t == -1 || dist[t] > dist[j]))
                t = j;

        for (int j = 1; j <= n; j ++ )
            dist[j] = min(dist[j], dist[t] + g[t][j]);

        st[t] = true;
    }

    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}

int main()
{
    scanf("%d%d", &n, &m);

    memset(g, 0x3f, sizeof g); //不用管自环，因为不会更新自身到自身d
    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);

        g[a][b] = min(g[a][b], c);
    }

    printf("%d\n", dijkstra());

    return 0;
}

```

堆优化版 如稀疏图

**邻接表**

```c++
#include <cstring>
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

typedef pair<int, int> PII;

const int N = 1e6 + 10;

int n, m;
int h[N], w[N], e[N], ne[N], idx;
int dist[N];
bool st[N];

void add(int a, int b, int c)
{
    e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
}

int dijkstra()
{
    memset(dist, 0x3f, sizeof dist);
    dist[1] = 0;
    priority_queue<PII, vector<PII>, greater<PII>> heap;
    heap.push({0, 1});

    while (heap.size())
    {
        auto t = heap.top();
        heap.pop();

        int ver = t.second, distance = t.first;

        if (st[ver]) continue; // 冗余备份
        st[ver] = true;

        for (int i = h[ver]; i != -1; i = ne[i])
        {
            int j = e[i];
            if (dist[j] > dist[ver] + w[i])
            {
                dist[j] = dist[ver] + w[i];
                heap.push({dist[j], j});
            }
        }
    }

    if (dist[n] == 0x3f3f3f3f) return -1;
    return dist[n];
}

int main()
{
    scanf("%d%d", &n, &m);

    memset(h, -1, sizeof h);
    while (m -- )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
    }

    printf("%d\n", dijkstra());

    return 0;
}

```

冗余备份的例子

在Dijkstra算法中，使用优先队列（或最小堆）来管理尚未处理的节点时，有时会出现所谓的“冗余备份”（redundant backups）情况。这种情况发生在某个节点已经被处理过（即最短路径已经确定），但由于优先队列的特性，它可能再次出现在队列中。

下面是一个详细解释和例子：

### 什么是冗余备份

冗余备份的情况发生在：

1. 节点已经被标记为访问过（其最短路径已经确定）。
2. 优先队列中仍然可能包含指向这个节点的其他路径信息。

在这种情况下，当我们从队列中取出这个节点时，发现它已经处理过，所以我们直接跳过这个节点。这就是所谓的冗余备份。

### 具体例子

假设我们有以下图：

```
节点: 1, 2, 3
边: (1 -> 2, 权重2), (1 -> 3, 权重5), (2 -> 3, 权重1)
```

图的表示：

```
rust复制代码1 --2--> 2 --1--> 3
 \        |
  \       |
   \      |
    \     |
     \    |
      \   |
       \  |
        \ |
         \|
          3 (权重5)
```

初始化：

- dist[1] = 0, dist[2] = inf, dist[3] = inf
- st[1] = false, st[2] = false, st[3] = false
- heap = [(0, 1)]

步骤：

1. 取出 (0, 1)：
   - 处理节点 1, 其邻居为 2 和 3。
   - 更新 dist[2] = 2, dist[3] = 5
   - 将 (2, 2) 和 (5, 3) 放入堆。
   - 标记 st[1] = true
   - heap = [(2, 2), (5, 3)]
2. 取出 (2, 2)：
   - 处理节点 2, 其邻居为 3。
   - 更新 dist[3] = 3 (因为 dist[3] > dist[2] + w[2->3])
   - 将 (3, 3) 放入堆。
   - 标记 st[2] = true
   - heap = [(3, 3), (5, 3)]
3. 取出 (3, 3)：
   - 处理节点 3。
   - 标记 st[3] = true
   - heap = [(5, 3)]
4. 取出 (5, 3)：
   - 发现节点 3 已经处理过，即 st[3] = true。
   - 跳过该节点（这就是冗余备份的情况）。
