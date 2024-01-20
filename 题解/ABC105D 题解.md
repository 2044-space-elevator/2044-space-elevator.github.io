#数学
[题目传送门。](https://www.luogu.com.cn/problem/AT_abc105_d)

> 建议管理员调低难度。感觉明显标高了。

## 大意

求满足以下情况的个数：

$$
m\mid \sum_{i=l}^r a_i
$$
也就是说，满足：

$$
\sum_{i=l}^r a_i =km
$$
两边模同一个模数相同，因此：

$$
\begin{aligned}
\left(\sum_{i=l}^ra_i\right)\bmod m &=0\\
\sum_{i=l}^r(a_i\bmod m)&=0\\
\end{aligned}
$$

对每一个 $a_i$ 重赋值为 $a_i \bmod m$，问题转换为，是否存在一种情况，使得：

$$
\sum_{i=l}^r a_i=0
$$

问题转换为：求前缀和数组中有多少个相同的值的组合。为什么要加个“的组合”呢？因为假设我么就考虑一种情况：就是整个数组里相同项只有一个，那么对应的，如果对于这个相同项有 $n$ 个数组里的数能匹配上，答案就相当于：把这些能匹配上的数两两组合，也就是一共有 $\dfrac{n(n-1)}{2}$ 个。最佳的数据结构当然是散列表。

另外，还有一个注意事项：散列表中 $0$ 得对应设成 $1$。

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
#define int ll
map<int, bool> mark;

void solve() {
    
}

#define int ll

main() {
    map<int, int> _M;
    int n, m;
    cin >> n >> m;
    int sum = 0;
    _M[0] = 1;
    rep(i, 1, n) {
        int x;
        cin >> x;
        sum += x;
        _M[sum % m]++;
    }    
    int ans = 0;
    for (auto [keyword, val] : _M) {
        ans += val * (val - 1) / 2; 
    }
    cout << ans;
    return 0;
}

```


