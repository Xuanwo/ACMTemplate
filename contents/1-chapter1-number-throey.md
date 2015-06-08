# 数论

## GCD
输入参数：两个数a，b
返回参数：a和b的最大公约数
时间复杂度：

```c++
int gcd(int a, int b)
{
    return b == 0 ? a : gcd(a%b ,a);
}
```
