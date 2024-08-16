[luogu RMJ 传送门。](https://www.luogu.com.cn/problem/UVA10310)

[UVA 传送门。](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=15&page=show_problem&problem=1251)

## 题面

有只狗和老鼠，狗的速度为老鼠两倍，给出初始坐标以及 $n$ 个老鼠洞，求老鼠在哪个洞能逃走。
## 坑点

1. 精度要求高，如何处理一会讲。
2. 多测，一定要把一个 case 里的所有数据读完再处理。
3. 可爱输出，UVA 传统艺能。

## 思路

距离公式：$\sqrt{(x_1-x_2)^2+(y_1-y_2)^2}$。

那么设狗的坐标为 $d(x,y)$，老鼠的为 $m(x,y)$，则有：对于每一个老鼠洞 $p(x,y)$，当 $\sqrt{(d_x-p_x)^2+(d_y-p_y)^2}\times 2\le \sqrt{(m_x-p_x)^2+(m_y-p_y)^2}$ 时，该老鼠洞可以帮助老鼠逃跑。

因为输入的是实数，处理过程中根号会难免出现精度缺失，因此在原来的式子对两边作平方，即判断 $(d_x-p_x)^2+(d_y-p_y)^2\times 4\le (m_x-p_x)^2+(m_y-p_y)^2$。

而要求输出编号最小的老鼠洞，所以找到解 `break` 即可。


## 代码

```cpp

```