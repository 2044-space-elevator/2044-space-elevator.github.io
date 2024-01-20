#字符串 #模拟
[博客园体验更好！](https://www.cnblogs.com/2044-space-elevator/articles/17857938.html)
## 思路

简单滴很，对于每一组 $(i,j)$ 找出其对应的三个点，减一减就完了。

对应的点是哪三个呢？显然是 $(n-i+1,n-j+1)$，$(j,n-i+1)$ 以及 $(i,n-j+1)$ 嘛！于是乎，键盘一挥，我写出了这些代码：
```cpp
// Problem: Perfect Square
// Contest: Luogu
// URL: https://www.luogu.com.cn/problem/CF1881C
// Memory Limit: 250 MB
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


char _map[(int)1e3 + 5][(int)1e3 + 5];
void solve() {
	int n;
	cin >> n;
	rep(i, 1, n) {
		rep(j, 1, n) {
			cin >> (_map[i][j] -= 'a');
		}
	}
	int res = 0;
	rep(i, 1, n >> 1) {
		rep(j, 1, n >> 1) {
			res += (
				_map[i][j] - map[j][n - i + 1]) + 
				_map[i][j] - _map[n - j + 1][i]) + 
				_map[i][j] - _map[n - i + 1][n - j + 1])
			);
		}
	}
	cout << res << endl;
}


int main() {
	int t; cin >> t; while (t--) solve();
	return 0;
}

```



结果样例没过……

## 哪里出问题了？

我们设这四个字母分别为 `e,c,d,f`，按照程序模拟一遍，可以得到为 $4$，这其实是因为你没法从 `f` 倒退到 `e`！这也是这个程序的错误之处。

所以我们只能从四个字母中选最大的，然后其他的三个字母一直加到最大的字母，所以我们可以得到正确的程序如下：

## 代码


```cpp
// Problem: Perfect Square
// Contest: Luogu
// URL: https://www.luogu.com.cn/problem/CF1881C
// Memory Limit: 250 MB
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


char _map[(int)1e3 + 5][(int)1e3 + 5];
void solve() {
	int n;
	cin >> n;
	rep(i, 1, n) {
		rep(j, 1, n) {
			cin >> (_map[i][j] -= 'a');
		}
	}
	int res = 0;
	rep(i, 1, n >> 1) {
		rep(j, 1, n >> 1) {
			initializer_list<int> tmp = {
				_map[i][j],
				_map[n - j + 1][i],
				_map[j][n - i + 1],
				_map[n - i + 1][n - j + 1]
			};
			res += max(tmp) * 4 - (accumulate(tmp.begin(), tmp.end(), 0));
		}
	}
	cout << res << endl;
}


int main() {
	int t; cin >> t; while (t--) solve();
	return 0;
}

```

> 解释一下：`initializer_list` 是我为了方便求值用的，其为函数中不定参数的类型。`accumulate` 为求和。

