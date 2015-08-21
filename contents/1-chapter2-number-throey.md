# 数论

## 欧几里得算法(GCD)

输入参数：两个数a，b

返回参数：a和b的最大公约数

```c++
int gcd(int a, int b)
{
    return b == 0 ? a : gcd(a%b ,a);
}
```

## 扩展欧几里得算法(exGCD)

输入参数：已知a，b，求一组x，y使得p*a+q*b=Gcd(a,b)

输出参数：r(r为零时，说明存在)

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

输出参数：a和b的最大公约数

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

输出参数：素数数组

```c++
const int NP = 1000005;
int ispri[NP] = {}, prime[NP], pcnt = 0;
void getprime()
{
    ispri[0] = ispri[1] = 1;
    for (long long i = 2; i < NP; i++)
        if (ispri[i] == 0)
        {
            prime[++pcnt] = i;
            for (long long j = i * i; j < NP; j += i)
                ispri[j] = 1;
        }
}
```

## 分解质因数

前提要求：
要求给出素数数组prime

输入参数：无

输出参数：
a数组 中存放质因数的种数

```c++
int a[1000005] = {};
void pdec(int n)
{
    int num = n;
    for (int i = 1; prime[i]*prime[i] <= n; i++)
        if (n % prime[i] == 0)
        {
            n /= prime[i];
            while (n % prime[i] == 0) n /= prime[i];
            a[num]++;
            if (n == 1) break;
        }
    if (n > 1) a[num]++;
}
```

## 快速幂

前置需求：const变量mod

输入参数：底数A，幂次n

输出参数：rslt（A的n次方）

```c++
ll qPow(ll a, ll b)
{
    if(b<0) return 0;
    ll ret = 1;
    a %= mod;
    for(; b; b>>=1,a=(a*a)%mod)
        if(b&1)
            ret = (ret*a)%mod;
    return ret;
}
```

## 求逆元（费马小定理）

前置需求：快速幂qPow

输入参数：待求数a，模数（必须为质数）mod

输出参数：这个数的逆元

```c++
ll inv(ll a)
{
    return qPow(a, mod-2);
}
```

