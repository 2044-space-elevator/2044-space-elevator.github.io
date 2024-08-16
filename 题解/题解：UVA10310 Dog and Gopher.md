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
#include <bits/stdc++.h>
#define rty printf("Yes\n");
#define RTY printf("YES\n");
#define rtn printf("No\n");
#define RTN printf("NO\n");
#define rep(v,b,e) for(int v=b;v<=e;v++)
#define repq(v,b,e) for(int v=b;v<e;v++)
#define rrep(v,e,b) for(int v=b;v>=e;v--)
#define rrepq(v,e,b) for(int v=b;v>e;v--)
#define stg string
#define vct vector
using namespace std;

typedef long long ll;
typedef unsigned long long ull;

struct Point {
  double x, y;
  double dist(const Point& b) const {
    return (x - b.x) * (x - b.x) + (y - b.y) * (y - b.y);
  }
};
istream & operator>> (istream &in, Point &A) {
  in >> A.x >> A.y;
  return in;
} // 友元函数，可以直接用 cin 输入结构体
int n;
void solve() {
  Point dog, mouse;
  cin >> mouse >> dog;
  bool flg = 0;
  rep(i, 1, n) {
    Point tmp;
    cin >> tmp;
    if (flg) continue;
    if (mouse.dist(tmp) * 4.0 <= dog.dist(tmp)) {
      cout << "The gopher can escape through the hole at (";
      cout << fixed << setprecision(3) << tmp.x << "," << tmp.y << ").\n";
      flg = 1;
    }
  } 
  if (!flg) cout << "The gopher cannot escape.\n";
}


main() {
  ios::sync_with_stdio(false);
    cin.tie(0);
	while (cin >> n) solve();
	return 0;
}
```