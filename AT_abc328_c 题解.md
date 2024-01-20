#字符串 #前缀和
## 题目大意

~~洛谷博客的体验更好哦。~~

给一个数 $|s|$、询问次数 $Q$、以及字符串 $s$，对于每一次询问，求：

在 $[l,r)$ 的区间内，有多少个位置满足 $s_i=s_{i+1}$。

## 暴力出奇迹——

很显然，这道题的暴力解法非常的简单，就是枚举每一个区间的字符串结果：

```cpp
// LUOGU_RID: 134656492
// Problem: [ABC328C] Consecutive
// Contest: Luogu
// URL: https://www.luogu.com.cn/problem/AT_abc328_c
// Memory Limit: 1 MB
// Time Limit: 2000 ms
// 
// Coding by 2044_space_elevator

#include <bits/stdc++.h>
using namespace std;

int tmp, Q, l, r, arr[(int)3e5 + 5];
string s;

int main() {
	cin >> tmp >> Q >> s;
	while (Q--) {
		cin >> l >> r;
		int ans = 0;
		for (int i = l; i < r; i++) {
			if (s[i - 1] == s[i]) {
				ans++;
			}
		}
		cout << ans << endl;
	}
}
```

但是，数据它不允许这样干啊，否则谁允许这道题放 ABC T3。[暴力代码只过了 8 个点，考虑优化](https://atcoder.jp/contests/abc328/submissions/47518031)。

## 何为前缀和？

比如说，我有 10 个数的数组 $a$，数组如下：

```
1 2 3 4 5 6 7 8 9 10
```

它们的前缀和就是：

```
1 3 6 10 15 21 28 36 45 55
```

前缀和的求法是 $S_i=S_{i-1}+a_i$，它的好处在于可以在 $\mathcal O(1)$ 的时间复杂度下做和的查询：比如，查询 $\sum\limits_{i=5}^7 a_i$，就不需要一个个去求了，只需要计算 $S_7-S_{5-1}$，得到 $18$。

## 本题如何运用前缀和？

我们可以先设数组 $sum$，$sum_i$ 表示前 $i$ 个串里有多少个情况满足 $s_i=s_{i+1}$，那么可以得到，区间 $[l,r)$ 内的结果就是 $s_{r-1}-s_{l-1}$。

所以我们可以得到最终代码如下：

```cpp
// LUOGU_RID: 134656492
// Problem: [ABC328C] Consecutive
// Contest: Luogu
// URL: https://www.luogu.com.cn/problem/AT_abc328_c
// Memory Limit: 1 MB
// Time Limit: 2000 ms
// 
// Coding by 2044_space_elevator

#include <bits/stdc++.h>
using namespace std;

int tmp, Q, l, r, arr[(int)3e5 + 5];
string s;

int main() {
	cin >> tmp >> Q >> s;
	for (int i = 0; i < s.size() - 1; i++) {
		arr[i + 1] = arr[i] + (s[i] == s[i + 1]);
	}
	while (Q--) {
		cin >> l >> r;
		cout << arr[r - 1] - arr[l - 1]<<endl;
	}
}
```
[AC 记录证明。](https://atcoder.jp/contests/abc328/submissions/47517854)