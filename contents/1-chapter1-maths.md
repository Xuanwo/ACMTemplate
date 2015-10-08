# 数学

## 高斯消元

复杂度：O(n^3)

输入参数：
a    方程组对应的矩阵
n    未知数个数
l    表示是否自由元
ans  存储答案
*默认下标从0开始*
a[i][n+1]存储常数项

返回参数：
*res表示解空间的维数*
res = -1, 表示无解。
res = 0, 表示有解。
res > 0, 表示有无数解。

```c++
const int maxn = 205;
double a[maxn][maxn] = {0}, ans[maxn] = {0};
bool l[maxn];
int n;
inline int solve(double a[][maxn], bool l[], double ans[], const int& n) {
    int res = 0, r = 0;
    for (int i = 0; i < n; i++) l[i] = false;
    for (int i = 0; i < n; i++) {
        for (int j = r; j < n; j++)
            if (fabs(a[j][i]) > eps) {
                for (int k = i; k <= n; k++) swap(a[j][k], a[r][k]);
                break;
            }
        if (fabs(a[r][i]) < eps) {
            res++;
            continue;
        }
        for (int j = 0; j < n; j++)
            if (j != r && fabs(a[j][i]) > eps) {
                double tmp = a[j][i] / a[r][i];
                for (int k = i; k <= n; k++) a[j][k] -= tmp * a[r][k];
            }
        l[i] = true, r++;
    }
    for (int i = r; i < n; i++) if (fabs(a[i][n]) > eps) return -1;
    for (int i = 0; i < n; i++)
        if (l[i])
            for (int j = 0; j < n; j++)
                if (fabs(a[j][i]) > eps)
                    ans[i] = a[j][n] / a[j][i];
    return res;
}
```

## long long相乘防溢出

输入参数：
a,b   相乘两数
mod   模数

返回参数：
(a*b)%mod

```c++
ll mult_mod(ll a,ll b,ll mod) //(a*b)%mod a,b,mod<2^63
{
    return (a*b-(ll)(a/(long double)mod*b+1e-3)*mod+mod)%mod;
}
```

