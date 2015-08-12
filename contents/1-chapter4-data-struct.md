# 数据结构

## RMQ

输入参数：
NV 数的个数
NVB 区间大小
mx[][] 区间最大值
mn[][] 区间最小值
a[] 存储数据

输出参数：
一个int，区间内的最大（小）值

### 一维RMQ

```c++
//下标从1开始
const int NV = 50005;
const int NVB = 20;
int mx[NV][NVB], mn[NV][NVB], a[NV];
void init(int data[], int n)
{
    int k = log2(n);
    for (int i = 1; i <= n; i++)
        mx[i][0] = mn[i][0] = data[i];
    for (int j = 1; j <= k; j++)
    {
        for (int i = 1; i + (1 << j) - 1 <= n; i++)
        {
            mx[i][j] = max(mx[i][j - 1], mx[i + (1 << (j - 1))][j - 1]);
            mn[i][j] = min(mn[i][j - 1], mn[i + (1 << (j - 1))][j - 1]);
        }
    }
}

int query(int l, int r, int flag)
{
    int k = log2(r - l + 1);
    if (flag)
        return max(mx[l][k], mx[r - (1 << k) + 1][k]);
    else
        return min(mn[l][k], mn[r - (1 << k) + 1][k]);
}
```

## 线段树

### 单点更新

```c++
#define lson l,m,rt<<1
#define rson m+1,r,rt<<1|1
const int NV = 100005;
int sum[NV << 2];
void PushUp(int rt)
{
    sum[rt] = sum[rt << 1] + sum[rt << 1 | 1];
}
void build(int l, int r, int rt = 1)
{
    if (l == r)
    {
        sum[rt] = 0;
        return ;
    }
    int m = (l + r) >> 1;
    build(lson);
    build(rson);
    PushUp(rt);
}
void update(int L, int c, int l, int r, int rt = 1)
{
    if (L == l && l == r)
    {
        sum[rt] += c;
        return ;
    }
    int m = (l + r) >> 1;
    if (L <= m) update(L , c , lson);
    else update(L , c , rson);
    PushUp(rt);
}
int query(int L, int R, int l, int r, int rt = 1)
{
    if (L <= l && r <= R)
        return sum[rt];
    int m = (l + r) >> 1;
    int ret = 0;
    if (L <= m) ret += query(L , R , lson);
    if (m < R) ret += query(L , R , rson);
    return ret;
}
```

### 区间更新

```
#define lson l,m,rt<<1
#define rson m+1,r,rt<<1|1
const int NV = 100005;
int add[NV << 2], sum[NV << 2];
void PushUp(int rt)
{
    sum[rt] = sum[rt << 1] + sum[rt << 1 | 1];
}
void PushDown(int rt, int m)
{
    if (add[rt])
    {
        add[rt << 1] += add[rt];
        add[rt << 1 | 1] += add[rt];
        sum[rt << 1] += add[rt] * (m - (m >> 1));
        sum[rt << 1 | 1] += add[rt] * (m >> 1);
        add[rt] = 0;
    }
}
void build(int l, int r, int rt = 1)
{
    add[rt] = 0;
    if (l == r)
    {
        sum[rt] = 0;
        return ;
    }
    int m = (l + r) >> 1;
    build(lson);
    build(rson);
    PushUp(rt);
}
void update(int L, int R, int c, int l, int r, int rt = 1)
{
    if (L <= l && r <= R)
    {
        add[rt] += c;
        sum[rt] += c * (r - l + 1);
        return ;
    }
    PushDown(rt , r - l + 1);
    int m = (l + r) >> 1;
    if (L <= m) update(L , R , c , lson);
    if (m < R) update(L , R , c , rson);
    PushUp(rt);
}
int query(int L, int R, int l, int r, int rt = 1)
{
    if (L <= l && r <= R)
    {
        return sum[rt];
    }
    PushDown(rt , r - l + 1);
    int m = (l + r) >> 1;
    int ret = 0;
    if (L <= m) ret += query(L , R , lson);
    if (m < R) ret += query(L , R , rson);
    return ret;
}
```

### 树上的线段树

```
const int NV = 10005;
const int NE = NV;
int he[NV], ecnt;
struct edge
{
    int v, next;
} E[NE];
void adde(int u, int v)
{
    ecnt++;
    E[ecnt].v = v;
    E[ecnt].next = he[u];
    he[u] = ecnt;
}
int l[NV], r[NV], p;
void dfs(int u)
{
    p++;
    l[u] = p;
    for (int i = he[u]; i != -1; i = E[i].next)
        dfs(E[i].v);
    r[u] = p;
}
void init()
{
    ecnt = p = 0;
    memset(he, -1, sizeof(he));
}
```

### 二维线段树区间更新单点求和

```
const int NV = 1005;
struct Nodey
{
    int l, r;
    int val;
};
int n;
int locx[NV], locy[NV];
struct Nodex
{
    int l, r;
    Nodey sty[NV * 3];
    void build(int _l, int _r, int i = 1)
    {
        sty[i].l = _l;
        sty[i].r = _r;
        sty[i].val = 0;
        if (_l == _r)
        {
            locy[_l] = i;
            return;
        }
        int mid = (_l + _r) >> 1;
        build(_l, mid, i << 1);
        build(mid + 1, _r, (i << 1) | 1);
    }
    void add(int _l, int _r, int val, int i = 1)
    {
        if (sty[i].l == _l && sty[i].r == _r)
        {
            sty[i].val += val;
            return;
        }
        int mid = (sty[i].l + sty[i].r) >> 1;
        if (_r <= mid)add(_l, _r, val, i << 1);
        else if (_l > mid)add(_l, _r, val, (i << 1) | 1);
        else
        {
            add(_l, mid, val, i << 1);
            add(mid + 1, _r, val, (i << 1) | 1);
        }
    }
} stx[NV * 3];
void build(int l, int r, int i = 1)
{
    stx[i].l = l;
    stx[i].r = r;
    stx[i].build(1, n);
    if (l == r)
    {
        locx[l] = i;
        return;
    }
    int mid = (l + r) >> 1;
    build(l, mid, i << 1);
    build(mid + 1, r, (i << 1) | 1);
}
void add(int x1, int x2, int y1, int y2, int val, int i = 1)
{
    if (stx[i].l == x1 && stx[i].r == x2)
    {
        stx[i].add(y1, y2, val);
        return;
    }
    int mid = (stx[i].l + stx[i].r) / 2;
    if (x2 <= mid)add(x1, x2, y1, y2, val, i << 1);
    else if (x1 > mid)add(x1, x2, y1, y2, val, (i << 1) | 1);
    else
    {
        add(x1, mid, y1, y2, val, i << 1);
        add(mid + 1, x2, y1, y2, val, (i << 1) | 1);
    }
}
int sum(int x, int y)
{
    int ret = 0;
    for (int i = locx[x]; i; i >>= 1)
        for (int j = locy[y]; j; j >>= 1)
            ret += stx[i].sty[j].val;
    return ret;
}
```

### 二维线段树单点更新区间求最大最小值

```
const int NV = 1005;
struct Nodey
{
    int l, r;
    int Max, Min;
};
int locy[NV], locx[NV];
struct Nodex
{
    int l, r;
    Nodey sty[NV * 4];
    void build(int _l, int _r, int i = 1)
    {
        sty[i].l = _l;
        sty[i].r = _r;
        sty[i].Max = -inf;
        sty[i].Min = inf;
        if (_l == _r)
        {
            locy[_l] = i;
            return;
        }
        int mid = (_l + _r) / 2;
        build(_l, mid, i << 1);
        build(mid + 1, _r, (i << 1) | 1);
    }
    int queryMin(int _l, int _r, int i = 1)
    {
        if (sty[i].l == _l && sty[i].r == _r)
            return sty[i].Min;
        int mid = (sty[i].l + sty[i].r) / 2;
        if (_r <= mid)return queryMin(_l, _r, i << 1);
        else if (_l > mid)return queryMin(_l, _r, (i << 1) | 1);
        else return min(queryMin(_l, mid, i << 1), queryMin(mid + 1, _r, (i << 1) | 1));
    }
    int queryMax(int _l, int _r, int i = 1)
    {
        if (sty[i].l == _l && sty[i].r == _r)
            return sty[i].Max;
        int mid = (sty[i].l + sty[i].r) / 2;
        if (_r <= mid)return queryMax(_l, _r, i << 1);
        else if (_l > mid)return queryMax(_l, _r, (i << 1) | 1);
        else return max(queryMax(_l, mid, i << 1), queryMax(mid + 1, _r, (i << 1) | 1));
    }
} stx[NV * 4];
int n;
void build(int l, int r, int i = 1)
{
    stx[i].l = l;
    stx[i].r = r;
    stx[i].build(1, n);
    if (l == r)
    {
        locx[l] = i;
        return;
    }
    int mid = (l + r) / 2;
    build(l, mid, i << 1);
    build(mid + 1, r, (i << 1) | 1);
}
void modify(int x, int y, int val)
{
    int tx = locx[x];
    int ty = locy[y];
    stx[tx].sty[ty].Min = stx[tx].sty[ty].Max = val;
    for (int i = tx; i; i >>= 1)
        for (int j = ty; j; j >>= 1)
        {
            if (i == tx && j == ty)continue;
            if (j == ty)
            {
                stx[i].sty[j].Min = min(stx[i << 1].sty[j].Min, stx[(i << 1) | 1].sty[j].Min);
                stx[i].sty[j].Max = max(stx[i << 1].sty[j].Max, stx[(i << 1) | 1].sty[j].Max);
            }
            else
            {
                stx[i].sty[j].Min = min(stx[i].sty[j << 1].Min, stx[i].sty[(j << 1) | 1].Min);
                stx[i].sty[j].Max = max(stx[i].sty[j << 1].Max, stx[i].sty[(j << 1) | 1].Max);
            }
        }
}
int queryMin(int x1, int x2, int y1, int y2, int i = 1)
{
    if (stx[i].l == x1 && stx[i].r == x2)
        return stx[i].queryMin(y1, y2);
    int mid = (stx[i].l + stx[i].r) / 2;
    if (x2 <= mid)return queryMin(x1, x2, y1, y2, i << 1);
    else if (x1 > mid)return queryMin(x1, x2, y1, y2, (i << 1) | 1);
    else return min(queryMin(x1, mid, y1, y2, i << 1), queryMin(mid + 1, x2, y1, y2, (i << 1) | 1));
}
int queryMax(int x1, int x2, int y1, int y2, int i = 1)
{
    if (stx[i].l == x1 && stx[i].r == x2)
        return stx[i].queryMax(y1, y2);
    int mid = (stx[i].l + stx[i].r) / 2;
    if (x2 <= mid)return queryMax(x1, x2, y1, y2, i << 1);
    else if (x1 > mid)return queryMax(x1, x2, y1, y2, (i << 1) | 1);
    else return max(queryMax(x1, mid, y1, y2, i << 1), queryMax(mid + 1, x2, y1, y2, (i << 1) | 1));
}
```


