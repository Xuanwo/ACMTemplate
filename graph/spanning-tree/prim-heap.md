# 最小生成树(Prim-堆)

## 接口
Type prim_heap(int n, int s)

复杂度：O(nlogn)

输入：
n 点数
s 起点位置
E[] 堆

输出：
pcnt 判断是否连通
ans 最小生成树的边权和
pre[] 树的构造，以父结点表示

## 代码
```
typedef double Type;

const int NV = 105;
const int NE = 10005 * 2;

Type dis[NV];
int pre[NV], vis[NV], he[NV], ecnt, pcnt;

struct edge
{
    int v, next;
    Type l;
} E[NE];

void adde(int u, int v, Type l)
{
    E[++ecnt].v = v;
    E[ecnt].l = l;
    E[ecnt].next = he[u];
    he[u] = ecnt;
}

void init(int n, int s)
{
    ecnt = 0;
    memset(pre, 0, sizeof(pre));
    memset(vis, 0, sizeof(vis));
    memset(he, -1, sizeof(he));
    for (int i = 0; i <= n; i++)
        dis[i] = inf;
    dis[s] = 0;
}

struct point
{
    int u;
    Type l;
    point(int a, Type b): u(a), l(b) {}
    bool operator<(const point p) const
    {
        return l > p.l;
    }
};

Type prim_heap(int n, int s)
{
    priority_queue<point> q;
    q.push(point(s, 0));
    Type ans = 0;
    pcnt = 0;
    while (!q.empty())
    {
        point p = q.top();
        q.pop();
        int u = p.u;
        if (vis[u])
            continue;
        vis[u] = 1;
        ans += p.l;
        pcnt++;
        for (int i = he[u]; i != -1; i = E[i].next)
            if (!vis[E[i].v] && E[i].l < dis[E[i].v])
            {
                dis[E[i].v] = E[i].l;
                pre[E[i].v] = u;
                q.push(point(E[i].v, dis[E[i].v]));
            }
    }
    return ans;
}

bool judge(int n)
{
    return pcnt == n;
}
```