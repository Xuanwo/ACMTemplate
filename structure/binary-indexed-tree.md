# 树状数组(Binary Indexed Tree)

# 功能
对数组A[n]，在O(logn)的时间内完成
1. 给A[i]加上一个数
2. 求A[1] + ··· +A[i]的和

# 接口
## void add(int x, int value)
复杂度：O(logn)

输入：
x, value A[x]增加value

## int get(int x)
复杂度：O(logn)

输入：
x 查询A[1]~A[x]的和

输出：
A[1] + ··· +A[x]

# 代码
```
const int maxn = 100000;
int Tree[maxn + 10];

inline int lowbit(int x)
{
    return (x & -x);
}

void add(int x, int value)
{
    for (int i = x; i <= maxn; i += lowbit(i))
        Tree[i] += value;
}

int get(int x)
{
    int sum = 0;
    for (int i = x; i; i -= lowbit(i))
        sum += Tree[i];
    return sum;
}
```