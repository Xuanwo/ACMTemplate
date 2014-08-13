# 最小生成树(kruskal邻接表)

```
//无向图最小生成树,kruskal算法,邻接表形式,复杂度O(mlogm)
//返回最小生成树的长度,传入图的大小n和邻接表list
//可更改边权的类型,edge[][2]返回树的构造,用边集表示
//如果图不连通,则对各连通分支构造最小生成树,返回总长度
#include <string.h>
#define MAXN 200
#define inf 1000000000
typedef double elem_t;
struct edge_t
{
    int from, to;
    elem_t len;
    edge_t *next;
};
#define _ufind_run(x) for(;p[t=x];x=p[x],p[t]=(p[x]?p[x]:x))
#define _run_both _ufind_run(i);_ufind_run(j)
struct ufind
{
    int p[MAXN], t;
    void init()
    {
        memset(p, 0, sizeof(p));
    }
    void set_friend(int i, int j)
    {
        _run_both;
        p[i] = (i == j ? 0 : j);
    }
    int is_friend(int i, int j)
    {
        _run_both;
        return i == j && i;
    }
};
#define _cp(a,b) ((a).len<(b).len)
struct heap_t
{
    int a, b;
    elem_t len;
};
struct minheap
{
    heap_t h[MAXN *MAXN];
    int n, p, c;
    void init()
    {
        n = 0;
    }
    void ins(heap_t e)
    {
        for (p = ++n; p > 1 && _cp(e, h[p >> 1]); h[p] = h[p >> 1], p >>= 1);
        h[p] = e;
    }
    int del(heap_t &e)
    {
        if (!n) return 0;
        for (e = h[p = 1], c = 2; c < n && _cp(h[c += (c < n - 1 && _cp(h[c + 1], h[c]))], h[n]); h[p] = h[c], p = c, c <<= 1);
        h[p] = h[n--]; return 1;
    }
};
elem_t kruskal(int n, edge_t *list[], int edge[][2])
{
    ufind u; minheap h;
    edge_t *t; heap_t e;
    elem_t ret = 0; int i, m = 0;
    u.init(), h.init();
    for (i = 0; i < n; i++)
        for (t = list[i]; t; t = t->next)
            if (i < t->to)
                e.a = i, e.b = t->to, e.len = t->len, h.ins(e);
    while (m < n - 1 && h.del(e))
        if (!u.is_friend(e.a + 1, e.b + 1))
            edge[m][0] = e.a, edge[m][1] = e.b, ret += e.len, u.set_friend(e.a + 1, e.b + 1);
    return ret;
}
```
