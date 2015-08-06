# 图论

## 最大流（效率较低）

输入参数：起点start、终点end、点的总数N

输出参数：结果ans

```c++
const int MAXN = 100010;//点数的最大值
const int MAXM = 400010;//边数的最大值
const int INF = 0x3f3f3f3f;
struct Edge
{
    int to, next, cap, flow;
} edge[MAXM]; //注意是MAXM
int tol;
int head[MAXN];
int gap[MAXN], dep[MAXN], pre[MAXN], cur[MAXN];
void init()
{
    tol = 0;
    memset(head, -1, sizeof(head));
}
//加边，单向边三个参数，双向边四个参数
void addedge(int u, int v, int w, int rw = 0)
{
    edge[tol].to = v;
    edge[tol].cap = w;
    edge[tol].next = head[u];
    edge[tol].flow = 0;
    head[u] = tol++;
    edge[tol].to = u;
    edge[tol].cap = rw;
    edge[tol].next = head[v];
    edge[tol].flow = 0;
    head[v] = tol++;
}
//输入参数：起点、终点、点的总数
//点的编号没有影响，只要输入点的总数（N的值只需大于点数之和）
int sap(int start, int end, int N)
{
    memset(gap, 0, sizeof(gap));
    memset(dep, 0, sizeof(dep));
    memcpy(cur, head, sizeof(head));
    int u = start;
    pre[u] = -1;
    gap[0] = N;
    int ans = 0;
    while (dep[start] < N)
    {
        if (u == end)
        {
            int Min = INF;
            for (int i = pre[u]; i != -1; i = pre[edge[i ^ 1].to])
                if (Min > edge[i].cap - edge[i].flow)
                    Min = edge[i].cap - edge[i].flow;
            for (int i = pre[u]; i != -1; i = pre[edge[i ^ 1].to])
            {
                edge[i].flow += Min;
                edge[i ^ 1].flow -= Min;
            }
            u = start;
            ans += Min;
            continue;
        }
        bool flag = false;
        int v;
        for (int i = cur[u]; i != -1; i = edge[i].next)
        {
            v = edge[i].to;
            if (edge[i].cap - edge[i].flow && dep[v] + 1 == dep[u])
            {
                flag = true;
                cur[u] = pre[v] = i;
                break;
            }
        }
        if (flag)
        {
            u = v;
            continue;
        }
        int Min = N;
        for (int i = head[u]; i != -1; i = edge[i].next)
            if (edge[i].cap - edge[i].flow && dep[edge[i].to] < Min)
            {
                Min = dep[edge[i].to];
                cur[u] = i;
            }
        gap[dep[u]]--;

        if (!gap[dep[u]])return ans;
        dep[u] = Min + 1;
        gap[dep[u]]++;
        if (u != start) u = edge[pre[u] ^ 1].to;
    }
    return ans;
}
```

## 最大流（效率更高）

最大流模板(SAP)

效率更高
起点和终点没有要求
传送的参数要大于总节点数量

```c++
#include<stdio.h>
#include<string.h>
#include<algorithm>
#include<iostream>
using namespace std;

const int MAXN = 100010; //点数的最大值
const int MAXM = 400010; //边数的最大值
const int INF = 0x3f3f3f3f;
struct Node
{
    int from, to, next;
    int cap;
} edge[MAXM];
int tol;
int head[MAXN];
int dep[MAXN];
int gap[MAXN];//gap[x]=y :说明残留网络中dep[i]==x的个数为y
void init()
{
    tol = 0;
    memset(head, -1, sizeof(head));
}
void addedge(int u, int v, int w)
{
    edge[tol].from = u;
    edge[tol].to = v;
    edge[tol].cap = w;
    edge[tol].next = head[u];
    head[u] = tol++;
    edge[tol].from = v;
    edge[tol].to = u;
    edge[tol].cap = 0;
    edge[tol].next = head[v];
    head[v] = tol++;
}
void BFS(int start, int end)
{
    memset(dep, -1, sizeof(dep));
    memset(gap, 0, sizeof(gap));
    gap[0] = 1;
    int que[MAXN];
    int front, rear;
    front = rear = 0;
    dep[end] = 0;
    que[rear++] = end;
    while (front != rear)
    {
        int u = que[front++];
        if (front == MAXN)front = 0;
        for (int i = head[u]; i != -1; i = edge[i].next)
        {
            int v = edge[i].to;
            if (dep[v] != -1)continue;
            que[rear++] = v;
            if (rear == MAXN)rear = 0;
            dep[v] = dep[u] + 1;
            ++gap[dep[v]];
        }
    }
}
int sap(int start, int end, int n)
{
    int res = 0;
    BFS(start, end);
    int cur[MAXN];
    int S[MAXN];
    int top = 0;
    memcpy(cur, head, sizeof(head));
    int u = start;
    int i;
    while (dep[start] < n)
    {
        if (u == end)
        {
            int temp = INF;
            int inser;
            for (i = 0; i < top; i++)
                if (temp > edge[S[i]].cap)
                {
                    temp = edge[S[i]].cap;
                    inser = i;
                }
            for (i = 0; i < top; i++)
            {
                edge[S[i]].cap -= temp;
                edge[S[i] ^ 1].cap += temp;
            }
            res += temp;
            top = inser;
            u = edge[S[top]].from;
        }
        if (u != end && gap[dep[u] - 1] == 0) //出现断层，无增广路
            break;
        for (i = cur[u]; i != -1; i = edge[i].next)
            if (edge[i].cap != 0 && dep[u] == dep[edge[i].to] + 1)
                break;
        if (i != -1)
        {
            cur[u] = i;
            S[top++] = i;
            u = edge[i].to;
        }
        else
        {
            int min = n;
            for (i = head[u]; i != -1; i = edge[i].next)
            {
                if (edge[i].cap == 0)continue;
                if (min > dep[edge[i].to])
                {
                    min = dep[edge[i].to];
                    cur[u] = i;
                }
            }
            --gap[dep[u]];
            dep[u] = min + 1;
            ++gap[dep[u]];
            if (u != start)u = edge[S[--top]].from;
        }
    }
    return res;
}
```

## 最小费用最大流

输入参数：
N 节点总个数，节点编号从0~N-1*（编号可以任意编号，N只需大于结点的最大编号）*

输出参数：
minCostMaxflow函数返回的是最大流，cost存的是最小费用

```c++
const int MAXN = 20000;
const int MAXM = 100000;
const int INF = 0x3f3f3f3f;
struct Edge
{
    int to, next, cap, flow, cost;
} edge[MAXM];
int head[MAXN], tol;
int pre[MAXN], dis[MAXN];
bool vis[MAXN];
int N;

void init(int n)
{
    N = n;
    tol = 0;
    memset(head, -1, sizeof(head));
}

void addedge(int u, int v, int cap, int cost)
{
    edge[tol].to = v;
    edge[tol].cap = cap;
    edge[tol].cost = cost;
    edge[tol].flow = 0;
    edge[tol].next = head[u];
    head[u] = tol++;
    edge[tol].to = u;
    edge[tol].cap = 0;
    edge[tol].cost = -cost;
    edge[tol].flow = 0;
    edge[tol].next = head[v];
    head[v] = tol++;
}
bool spfa(int s, int t)
{
    queue<int>q;
    for (int i = 0; i < N; i++)
    {
        dis[i] = INF;
        vis[i] = false;
        pre[i] = -1;
    }
    dis[s] = 0;
    vis[s] = true;
    q.push(s);
    while (!q.empty())
    {
        int u = q.front();
        q.pop();
        vis[u] = false;
        for (int i = head[u]; i != -1; i = edge[i].next)
        {
            int v = edge[i].to;
            if (edge[i].cap > edge[i].flow &&
                    dis[v] > dis[u] + edge[i].cost )
            {
                dis[v] = dis[u] + edge[i].cost;
                pre[v] = i;
                if (!vis[v])
                {
                    vis[v] = true;
                    q.push(v);
                }
            }
        }
    }
    if (pre[t] == -1)return false;
    else return true;
}

int minCostMaxflow(int s, int t, int &cost)
{
    int flow = 0;
    cost = 0;
    while (spfa(s, t))
    {
        int Min = INF;
        for (int i = pre[t]; i != -1; i = pre[edge[i ^ 1].to])
        {
            if (Min > edge[i].cap - edge[i].flow)
                Min = edge[i].cap - edge[i].flow;
        }
        for (int i = pre[t]; i != -1; i = pre[edge[i ^ 1].to])
        {
            edge[i].flow += Min;
            edge[i ^ 1].flow -= Min;
            cost += edge[i].cost * Min;
        }
        flow += Min;
    }
    return flow;
}
```

