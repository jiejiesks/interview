https://www.acwing.com/activity/content/problem/content/922/、

Bellman - ford 算法是求含负权图的单源最短路径的一种算法，效率较低，代码难度较小。其原理为连续进行松弛，在每次松弛时把每条边都更新一下，若在 n-1 次松弛后还能更新，则说明图中有负环，因此无法得出结果，否则就完成。
(通俗的来讲就是：假设 1 号点到 n 号点是可达的，每一个点同时向指向的方向出发，更新相邻的点的最短距离，通过循环 n-1 次操作，若图中不存在负环，则 1 号点一定会到达 n 号点，若图中存在负环，则在 n-1 次松弛后一定还会更新



如果存在负权重，那么是否能到达n号点的判断中需要进行if(dist[n] > INF/2)判断，而并非是if(dist[n] == INF)判断，原因是INF是一个确定的值，并非真正的无穷大，会随着其他数值而受到影响，dist[n]大于某个与INF相同数量级的数即可比如（INF -2）

5号节点距离起点的距离是无穷大，利用5号节点更新n号节点距离起点的距离，将得到$10^9−2$, 但并不存在最短路，(在边数限制在k条的条件下)。

![4.PNG](https://jiejiesks.oss-cn-beijing.aliyuncs.com/Note/202407021454014.png)

```c++
#include <cstring>
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 510, M = 10010;

struct Edge
{
    int a, b, c;
}edges[M];

int n, m, k;
int dist[N];
int last[N];

void bellman_ford()
{
    memset(dist, 0x3f, sizeof dist);

    dist[1] = 0;
    for (int i = 0; i < k; i ++ )
    {
        memcpy(last, dist, sizeof dist);
        for (int j = 0; j < m; j ++ )
        {
            auto e = edges[j];
            dist[e.b] = min(dist[e.b], last[e.a] + e.c);
        }
    }
}

int main()
{
    scanf("%d%d%d", &n, &m, &k);

    for (int i = 0; i < m; i ++ )
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        edges[i] = {a, b, c};
    }

    bellman_ford();

    if (dist[n] > 0x3f3f3f3f / 2) puts("impossible");
    else printf("%d\n", dist[n]);

    return 0;
}
```

