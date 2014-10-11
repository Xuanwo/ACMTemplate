# 无向图关键点(dfs邻接阵)

```C/C++
//无向图的关键点,dfs邻接阵形式,O(n^2)
//返回关键点个数,key[]返回点集
//传入图的大小n和邻接阵mat,不相邻点边权0
#define MAXN 110
void search(int n, int mat[][MAXN], int *dfn, int *low, int now, int &ret, int *key, int &cnt, int root, int &rd, int *bb)
{
    int i;
    dfn[now] = low[now] = ++cnt;
    for (i = 0; i < n; i++)
        if (mat[now][i])
        {
            if (!dfn[i])
            {
                search(n, mat, dfn, low, i, ret, key, cnt, root, rd, bb);
                if (low[i] < low[now])
                    low[now] = low[i];
                if (low[i] >= dfn[now])
                {
                    if (now != root && !bb[now])
                        key[ret++] = now, bb[now] = 1;
                    else if (now == root)
                        rd++;
                }
            }
            else if (dfn[i] < low[now])
                low[now] = dfn[i];
        }
}
int key_vertex(int n, int mat[][MAXN], int *key)
{
    int ret = 0, i, cnt, rd, dfn[MAXN], low[MAXN], bb[MAXN];
    for (i = 0; i < n; dfn[i++] = bb[i] = 0);
    for (cnt = i = 0; i < n; i++)
        if (!dfn[i])
        {
            rd = 0;
            search(n, mat, dfn, low, i, ret, key, cnt, i, rd, bb);
            if (rd > 1 && !bb[i])
                key[ret++] = i, bb[i] = 1;
        }
    return ret;
}

```