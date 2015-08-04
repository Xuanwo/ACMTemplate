# 计算几何

## 叉积

输入参数： 三个点a，b，c

输出参数：
向量ab X 向量ac 的值

- >0 : 表示ac在ab的顺时针方向
- ==0 : 表示ac与ab平行或重合
- <0 : 表示ac在ab的逆时针方向

```c++
double crossProduct(node a, node b, node c)
{
    return (c.x - a.x) * (b.y - a.y) - (b.x - a.x) * (c.y - a.y);
}
```

