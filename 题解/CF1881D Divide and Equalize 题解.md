#数论 #数学
## 题目大意

每次操作任意选择三个正整数 $a_i$ ，$a_j$ 以及 $x$（$x|a_i$），将 $a_i$ 赋值为 $\frac{a_i}{x}$ 并将 $a_j$ 赋值为 $a_j\cdot x$。请问是否能通过若干次操作，使得 $a$ 数组任意两个数相等？

## 思路

首先我们得明白一个铁打不动的规则：就是无论你怎么操作，$a$ 数组的积不变，也就是说 $\prod_{i=1}^n a_i$ 的值是不变的。那么我们可以得出，答案就是：

$$
\sqrt[n]{\prod_{i=1}^n a_i} \in \mathbf{Z}
$$
是否为真，可是题目里 $a_i\le 10^6$，$n\le 10^4$，乘几下 `__int128` 就炸了，所以该方法行不通。

## 质因数是个好东西

我们将所有 $a_i$ 质因数分解：

$$
\sqrt[n]{\prod_{i=1}^k p_i^{c_i}}
$$
其中根号里的式子是 $a$ 的积的质因数分解形式。

所以现在就可以简化问题了，其实就是求：

$$
\forall n|c_i
$$
是真是否。那么现在要做的就简单了，我们只需要将所有 $a_i$ 质因数分解，将质因数的次数放入散列表里面映射，散列表里的值如果都能被 $n$ 整除，答案为真，否则为否。

代码很简单，我还用了我自己的模板，欢迎自取。

```cpp
// Problem: D. Divide and Equalize
// Contest: Codeforces - Codeforces Round 903 (Div. 3)
// URL: https://codeforces.com/problemset/problem/1881/D
// Memory Limit: 256 MB
// Time Limit: 2000 ms
// 
// Coding by 2044_space_elevator

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
using namespace std;

typedef long long ll;
typedef unsigned long long ull;

void solve() {
	map<int, int> primeMap;
	int n;
	cin >> n;
	rep(i, 1, n) {
		int x;
		cin >> x;
		rep(i, 2, sqrt(x)) {
			while (x % i == 0) {
				primeMap[i]++;
				x /= i;
			}
		}
		primeMap[x] += x > 1;
	}
	for (auto [trash, useful] : primeMap) {
		if (useful % n) {
			RTN
			return;
		}
	}
	RTY
	return;
}


int main() {
	int t; cin >> t; while (t--) solve();
	return 0;
}

```