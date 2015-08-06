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

