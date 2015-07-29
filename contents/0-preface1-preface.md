# 前言
这是TopIdiots队的ACM-ICPC代码模板。

## 格式要求
1. 所有代码要求使用AStyleFormat进行格式化。
1. 所有模板需给出输入参数以及输出参数。
1. 算法有中文译名的，应以中文译名为标题，英文名以加括号形式跟在中文译名后。
1. 由于使用了XeLaTex进行编译，所以所有的代码块需表明使用`c++`，否则无法高亮。
1. 所有函数名，变量名力求有意义，便于理解。
1. 对其他函数有依赖的，应当在代码最开始以注释的形式标明。
1. 代码关键处应当附有简单的注释，以便于理解和使用为标准。
1. 结构体类型均使用node命名，使用到多个结构体的，则在node后加上数字序号。而在使用结构体时，应当尽可能使用有意义的名字。

## 使用须知
1. 所有函数要求为整型的，均返回int；要求为浮点型的，均返回double。使用时请自行修改。

## 头文件
```c++
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <cmath>
#include <ctime>
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>
#include <deque>
#include <list>
#include <set>
#include <map>
#include <stack>
#include <queue>
#include <numeric>
#include <iomanip>
#include <bitset>
#include <sstream>
#include <fstream>
#define debug puts("-----")

typedef long long int ll;
const double pi = acos(-1.0);
const double eps = 1e-8;
const int inf = 0x3f3f3f3f;
const ll INF = 0x3f3f3f3f3f3f3f3fLL;
using namespace std;

int main()
{
    ios::sync_with_stdio(false);
    return 0;
}
```