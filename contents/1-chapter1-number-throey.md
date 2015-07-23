# 数论

## 欧几里得算法(GCD)

输入参数：两个数a，b

返回参数：a和b的最大公约数

时间复杂度：

```c++
int gcd(int a, int b)
{
    return b == 0 ? a : gcd(a%b ,a);
}
```

## 扩展欧几里得算法(exGCD)

输入参数：已知a，b，求一组x，y使得p*a+q*b=Gcd(a,b)

返回参数：r(r为零时，说明存在)

时间复杂度：

```c++
int exGcd(int a, int b, int &x, int &y)
{
    if(b == 0)
    {
        x = 1;
        y = 0;
        return a;
    }
    int r = exGcd(b, a % b, x, y);
    int t = x;
    x = y;
    y = t - a / b * y;
    return r;
}
```

## Stein算法

输入参数：两个数a，b

返回参数：a和b的最大公约数

时间复杂度：

```c++
int Gcd(int a, int b)
{
    if(a == 0) return b;
    if(b == 0) return a;
    if(a % 2 == 0 && b % 2 == 0) return 2 * gcd(a >> 1, b >> 1);
    else if(a % 2 == 0)  return gcd(a >> 1, b);
    else if(b % 2 == 0) return gcd(a, b >> 1);
    else return gcd(abs(a - b), Min(a, b));
}
```

## 快速筛法求素数

输入参数：无

返回参数：素数数组

时间复杂度：

```c++
const long N = 200000;
long prime[N] = {0}, num_prime = 0;
int isNotPrime[N] = {1, 1};
int main()
{
    for (long i = 2 ; i < N ; i ++)
    {
        if (! isNotPrime[i])
            prime[num_prime ++] = i;
        for (long j = 0 ; j < num_prime && i * prime[j] <  N ; j ++)
        {
            isNotPrime[i * prime[j]] = 1;
            if ( !(i % prime[j] ) )
                break;
        }
    }
    return 0;
}
```

## 快速幂

输入参数：底数A，幂次n

返回参数：rslt（A的n次方）

时间复杂度：

```c++
int qPow(int A, int n)
{
    if (n == 0) return 1;
    int rslt = 1;

    while (n)
    {
        if (n & 1) //如果n为奇数
        {
            rslt *= A;
        }
        A *= A;
        n >>= 1;
    }
    return rslt;
}
```

