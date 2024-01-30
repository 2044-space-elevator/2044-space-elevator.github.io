#数学 

好久没写题解了，再不写就要掉橙了。这里给大家[安利我的博客](https://www.cnblogs.com/2044-space-elevator)。

题目想让我们求的是：

$$
\frac{n}{\sum\limits_{i=1}^n\frac{1}{a_i}}
$$

先不看上面的 $n$，只看：

$$
\sum^{n}_{i=1}\frac{1}{a_i}
$$


那么我们先回顾一下小学里学的通分，也就是说：

$$
\frac{c}{d}+\frac{d}{b} = \frac{ad+bc}{ab}=\frac{\frac{ad+bc}{\gcd(ab,ad+bc)}}{\frac{ab}{\gcd(ab,ad+bc)}}
$$
现在就好办了，但是因为数据十分巨大，所以要边遍历边约分，还需要开 `long long`。在考虑完下面的部分以后，考虑上面的，将分子的结果记作 $\dfrac{q}{p}$。现在要求的式子为：

$$
\frac{n}{\frac{p}{q}}
$$
两边乘以 $q$，得到 $\frac{nq}{p}$，接着两边再做约分即可。不得不说 UVa 输出真的是麻烦。代码如下：

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
#define int ll
using namespace std;

typedef long long ll;
typedef unsigned long long ull;

int cas = 0;

void solve() {
  cas++;
  int n;
  int q = 0, p = 1;
  // 分母设 1 分子设 0
  cin >> n;
  rep(i, 1, n) {
    int x;
    cin >> x;
    q = p + q * x;
    p *= x;
    // 通分
    int tmp = __gcd(p, q);
    p /= tmp; q/= tmp;
  }
  int tmp = __gcd(p * n, q); 
  // 最后要通分
  // 不要写 %d！！
  printf("Case %lld: %lld/%lld\n", cas, p * n / tmp, q/ tmp);
}


main() {
	int t; cin >> t; while (t--) solve();
	return 0;
}
```