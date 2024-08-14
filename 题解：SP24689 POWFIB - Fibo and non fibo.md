## 提示

- 能开 `ll` 的开 `ll`。
- 矩阵快速幂的初始化，不要偷懒，要写全！
- 对于第一个解，斐波那契数列传递到 $100$ 够了，此时远大于 $10^9+7$。

[我的惨痛经历。](https://www.luogu.com.cn/record/list?user=824363&pid=SP24689&page=1)

## 问一

正常的递推，$f_i=f_{i-1}+f_{i-2}$，无需多言。设一个数 $a=n$，**因为第 $n$ 个不是斐波那契数列的数一定大于等于 $n$。**

## 问二

这时候需要讲一下矩阵快速幂。
### 关于矩阵快速幂那些事
#### 矩阵乘法

对于 $\rm C=AB$，有：

$$
\mathrm{C}_{i,j}=\sum_{i=1}^n\mathrm{A}_{i,k}\mathrm{B}_{k,j}
$$

#### 构造矩阵

首先构造：

$$
\left[\begin{array}{c}
F_{n}\\
F_{n-1}
\end{array}\right]
=\mathrm{A}\left[\begin{array}{c}
F_{n-1}\\
F_{n-2}
\end{array}\right]
$$

因为有：

$$
F_n=F_{n - 1}+F_{n - 2}
$$
所以，根据矩阵快速幂的相关性质，能得到：

$$
\color{red}
\mathrm{A}=\left[\begin{array}{c}
1 & 1\\
1 & 0
\end{array}\right]
$$

因此，有初始状态：

$$
\left[\begin{array}{c}
F_{2}\\
F_{1}
\end{array}\right]=\left[\begin{array}{c}
1\\
1
\end{array}\right]
$$
而这样，我们就能得到一条十分关键的定理：

$$
\color{red}
\left[\begin{array}{c}
F_{n}\\
F_{n-1}
\end{array}\right]=\mathrm{A}^{n-2}\left[\begin{array}{c}
F_{n-1}\\
F_{n-2}
\end{array}\right]
$$

#### 矩阵快速幂

与普通快速幂一样，无太大区别。

#### 模板

```cpp
struct Matrix {
  ll m[5][5];
  Matrix() {
    memset(m, 0, sizeof(m));
  }
  Matrix operator*(const Matrix& b) const {
    Matrix rns;
    rep(i, 1, 2) { rep(j, 1, 2) { rep(k, 1, 2) {
      rns.m[i][j] += m[i][k] % MOD * b.m[k][j] % MOD;
      rns.m[i][j] %= MOD;
    } } }
    return rns;
  }
}ans, base;
```

### 代码

以上该讲的都讲了，现在是大家最期待的代码：

```cpp
#include <bits/stdc++.h>
#define rty printf("Yes\n");
#define RTY printf("YES\n");
#define rtn printf("No\n");
#define RTN printf("NO\n");
typedef long long ll;
#define int ll
#define rep(v,b,e) for(int v=b;v<=e;v++)
#define repq(v,b,e) for(int v=b;v<e;v++)
#define rrep(v,e,b) for(int v=b;v>=e;v--)
#define rrepq(v,e,b) for(int v=b;v>e;v--)
#define stg string
#define vct vector
using namespace std;

typedef unsigned long long ull;

int NormalFib[(int)1e7 + 7];

const ll MOD = 1e9 + 7;

struct Matrix {
  ll m[5][5];
  Matrix() {
    memset(m, 0, sizeof(m));
  }
  Matrix operator*(const Matrix& b) const {
    Matrix rns;
    rep(i, 1, 2) { rep(j, 1, 2) { rep(k, 1, 2) {
      rns.m[i][j] += m[i][k] % MOD * b.m[k][j] % MOD;
      rns.m[i][j] %= MOD;
    } } }
    return rns;
  }
}ans, base;

void MatrixInit() {
  base.m[1][1] = base.m[1][2] = base.m[2][1] = 1;
  base.m[2][2] = ans.m[2][1] = ans.m[2][2] = 0;
  ans.m[1][1] = ans.m[1][2] = 1;
}

void qpow(ll b) {
  while (b) {
    if (b & 1) ans = ans * base;
    base = base * base;
    b >>= 1;
  }
}
void NormalFibInit() {
  NormalFib[0] = NormalFib[1] = 1;
  rep(i, 2, INT_MAX) {
    NormalFib[i] = NormalFib[i - 1] + NormalFib[i - 2];
    if (NormalFib[i] >= MOD) {
      break;
    }
  }
}

ll fastpow(ll a, ll b) {
  ll r = 1;
  while (b) {
    if (b & 1) r *= a % MOD;
    a = a * a % MOD;
    a %= MOD;
    r %= MOD;
    b >>= 1;
  }
  return r % MOD;
}

void solve() {
  MatrixInit();
  ll n;
  cin >> n;
  ll a = n;
  rep(i, 1, 100) {
    if (NormalFib[i] >= MOD) {
      break;
    }
    if (a >= NormalFib[i]) a++; else break;
  }
  ll b;
  if (n >= 2) {
    qpow(n - 2);
    b = ans.m[1][1];
  } else b = 1;
  cout << fastpow(a, b) << '\n';

}


main() {
  NormalFibInit();
	int t; cin >> t; while (t--) solve();
	return 0;
}

```