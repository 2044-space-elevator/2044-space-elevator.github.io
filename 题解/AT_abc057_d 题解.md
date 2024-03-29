#贪心  

[题目传送门。](https://www.luogu.com.cn/problem/AT_abc057_d)

于 `2023/08/01 13:18` 写。

## 前言

这题目坑还是有点多的，我就是因为变量类型问题 WA 了 7 次，我把坑整理一下：

- 不要开 `double`，`double` 是存不下除样例以外的数据的，也不要开 `long double`，因为一些玄学原因，开了 `long double` 会爆零，想办法把所有的 `long double` 都换成 `long long`。平均值处理这块可以先把平均值用 `long long` 累加，再强转 `double` 然后做除法。
- 尽量不要开 `int`，把所有的 `int` 都换成 `long long`。
- 很多代码角落的地方是你注意不到的，所以为了避免多次爆零，可以用宏定义把所有 `int` 提前都转成 `long long`。

## 贪心策略

看到算法标签，大家应该都能猜到现需要把数组给排序，为什么要排序呢？

排序后前 $A$ 个元素是必定要选的。为什么？因为前 $A$ 个元素肯定是最大的那 $A$ 个元素，而我们希望尽量在保证平均值单调递增的情况下，选择的元素最少，那么肯定要先选前 $A$ 个元素。

那对于第二小问呢？

有一个临界值，前文提到，选择前 $A$ 个元素是最优选择，那么临界值就是 $v_A$。 设在区间 $[1,A]$ 内有 $k_1$ 个元素使得 $v_i=v_A$，在整个数组中，有 $k_2$ 个元素使得 $v_i=v_A$ 在这种情况下，第二小问的答案是：

$$
C^{k_1}_{k_2}
$$


这个式子的含义是什么？是表示从那 $k_2$ 个数选择 $k_1$ 个数有多少种方案。这应该很好理解，我也不过多赘述了。

如果你现在就急着去写，恭喜你，喜提部分分。

为什么？因为我们还没有考虑到一种特殊情况，那就是 $v_1=v_A$，这种情况我们只需要选不影响平均值即可，只要选的数的个数不超过 $B$，那么，这一部分的公式如下：

$$
\sum_{i=A}^BC^i_{k_1}
$$

所以最终代码如下：

```cpp
#include <algorithm>
#include <cstdio>
#include <iostream>
using namespace std;
long long YangHuiTraingle[55][55]; // 杨辉三角，排列组合用
void init()
{
    YangHuiTraingle[0][0] = 1;
    for (int i = 1; i <= 50; i++)
    {
        YangHuiTraingle[i][0] = 1;
        for (int j = 1; j <= i; j++)
        {
            YangHuiTraingle[i][j] = YangHuiTraingle[i - 1][j] + YangHuiTraingle[i - 1][j - 1];
        }
    }
}
long long arr[114514];
int n, A, B;
long long average = 0.0; // 平均值
int main()
{
    init();
    cin >> n >> A >> B;
    for (int i = 1; i <= n; i++)
    {
        cin >> arr[i];
    }  
    // 排序，注意 greater要用 long long
    sort(arr + 1, arr + n + 1, greater<long long>());
    long long choose = 0; // 选择了多少元素？
    long long choosebefore = 0; // 表示 k_1
    long long chooseafter = 0; // 表示 k_2
    for (int i = 1; i <= n; i++)
    {
    // 这是刚开始写错的，不用过多理会
        if (choose < A)
        {
            average += arr[i];
            choose++;
        }
        else if (choose > B)
        {
            break;
        }
        else if (choose >= A && arr[i] > average)
        {
            average += arr[i];
            choose++;
        }
    }
    for (int i = 1; i <= n; i++)
    {
        if (arr[i] == arr[A])
        {
            choosebefore++;
            if (i <= A)
            {
                chooseafter++;
            }
        }
    }
    long long ans2 = 0;
    if (arr[1] == arr[A])
    
    {
        for (int i = A; i <= B; i++)
        {
            ans2 += YangHuiTraingle[choosebefore][i];
        }
    }
    else
    {
        ans2 += YangHuiTraingle[choosebefore][chooseafter];
    }
    printf("%.6lf\n%lld", (average / (double)(choose)), ans2);
}
```
[AC记录证明。](https://atcoder.jp/contests/abc057/submissions/44153230)
希望大家愉快的 AC 此题。