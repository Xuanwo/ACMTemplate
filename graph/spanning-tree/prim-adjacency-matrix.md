# 最小生成树(Prim-邻接阵)

## [接口]
Type prim(int n);

复杂度：O(n^2)

输入：
n 边数
s 起点位置
dis[] 边权
g[][] 邻接阵

输出：
cnt 判断是否连通
ans 最小生成树的边权和
pre[] 树的构造，以父结点表示

## [代码]
```
typedef int Type;
const int NV = 1005;
int pre[NV], vis[NV];
Type dis[NV], g[NV][NV];

void init(int n, int s)
{
    memset(pre, 0, sizeof(pre));
    memset(vis, 0, sizeof(vis));
    for (int i = 0; i <= n; i++)
        dis[i] = inf;
    dis[s] = 0;
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            scanf("%d", &g[i][j]);
}

Type prim(int n)
{
    Type ans = -1;
    for (int i = 1; i <= n; i++)
    {
        int u = 0;
        for (int j = 1; j <= n; j++)
            if (!vis[j] && dis[j] < dis[u])
                u = j;
        vis[u] = 1;
        ans = max(ans, dis[u]);
        for (int j = 1; j <= n; j++)
            if (!vis[j] && g[u][j] < dis[j])
            {
                dis[j] = g[u][j];
                pre[j] = u;
            }
    }
    return ans;
}

bool judge(int n)
{
    int cnt = 0;
    for (int i = 1; i <= n; i++)
        cnt += vis[i];
    return cnt == n;
}
```