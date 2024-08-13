## 介绍

首先希望大家点赞评论，谢谢！

模拟退火是一种随机算法，由为了使模拟退火能被使用，必须满足答案只与顺序有关。

这是一种随机性算法，适用于普通爆搜用不了的情况。其抽象在要不断的取随机数，所以这种算法最核心的地方在于乱取随机数，直到达到最优解。

听上去很抽象是吗？但是它的随机也是有规律的随机，我们将不断的重复退火的过程，单次退火后随机转移的概率会越来越低。**这就是 SA 从不稳定到稳定的过程**。

## 算法过程

我们要有个初始温度 $T$，热力学满分的童鞋都知道，$T$ 越高的时候，粒子也就越不稳定。

[OI-WIKI 上的描述稍显专业，个人觉得本文所述更易理解一点。大家也可以参考](https://oi-wiki.org/misc/simulated-annealing/#%E8%BF%87%E7%A8%8B)

以排列最优问题为例，SA 的单次迭代，会随机选择两个初始排列的数，交换它们。调用 `get` 函数做计算：有两种情况，在较之前已有情况更优时，直接修改答案，并且保留已经交换的数。如果不优，那么以一定的概率保持之前修改了的状态，这个概率是多少呢？我们设 $\rm tmp$ 为此次的答案，$\rm ans$ 为目前最优的答案。那么这里 $\Delta E=\rm ans- tmp$，即能量差，则有：$\mathrm{e}^{-\frac{\Delta E}{T}}$ 的转移概率。

单词迭代完成后，$T$ 要乘以一个小于 $1$ 的固定系数，即“退火”，等待下一次迭代。

迭代到什么时候结束呢？迭代当 $T$ 达到一个十分接近 $0$ 的值时结束。

## 模拟退火模板

单说可能会有点抽象，一下是模板代码：

```cpp
double Rand() {
  // 可以获得小于等于 1 的随机数
  return (double)rand() / RAND_MAX;
}

void sa() {
  double t = 1000; // 可以根据需要修改
  while (t > 1e-12) { // 可以根据需要修改
    // 随机转移
    double tmp = get(); // 自设的计算函数
    if (tmp < ans) {
      ans = tmp; // 默认转移，并设 ans 为目前最优解
    } else if (exp((ans - tmp) / t) > Rand()){
      // 随机下来无法转移，撤销转移
    }
    t *= 0.996; // 可以根据需要修改
  }
}

```

## 卡时技巧

为了保证答案准确性，一般要多做很多次 SA，以下代码可以保证不超时的情况下达到正确率最大化：

```cpp
while((double)clock() / CLOCKS_PER_SEC <= time_limit) sa();
// time_limit 为题目限时，建议预留 100ms - 200ms，防止出现超时
```
## 习题

最近在做油滴扩展，本是一道可以暴力水过的题，但还是给大家看一下代码，Luogu P1378：

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

void solve() {
	
}

const double pi = 3.141592653589793;
const double e = 2.718281;

int n, zx, yx, sy, xy;
int objx[10], objy[10];
double radius[10];
double ans = 0;

double dis(double x1, double y1, double x2, double y2) {
  return sqrt((x1 - x2)*(x1 - x2) + (y1 - y2)*(y1 - y2));
}

double Rand() {
  return (double)rand() / RAND_MAX;
}

double get() {
  double sum = 0;
  rep(i, 1, n) {
    radius[i] = 
      min(
        {
          abs(yx - objx[i]),
          abs(zx - objx[i]),
          abs(sy - objy[i]),
          abs(xy - objy[i])
        }
      );
    repq(j, 1, i) {
      double di = dis(objx[i], objy[i], objx[j], objy[j]);
      if (di >= radius[j]) { 
        radius[i] = min(radius[i], di - radius[j]);
      } else radius[i] = 0;
    }
    sum += radius[i] * radius[i] * pi;
  }
  return abs(yx - zx) * abs(sy - xy) - sum; 
}

void sa() {
  double t = 1000;
  while (t > 1e-12) {
    int a = rand() % n + 1;
    int b = rand() % n + 1;
    swap(objx[a], objx[b]); swap(objy[a], objy[b]);
    double tmp = get();
    if (tmp < ans) {
      ans = tmp;
    } else if (pow(e, (ans - tmp) / t) < Rand()){
      swap(objx[a], objx[b]); swap(objy[a], objy[b]);
    }
    t *= 0.996;
  }
}

main() {
//	int t; cin >> t; while (t--) solve();
  srand(time(NULL));
  cin >> n; 
  cin >> zx >> sy >> yx >> xy;
  if (yx > zx) swap(yx,zx);
  if (sy > xy) swap(sy, xy);
  rep(i, 1, n) cin >> objx[i] >> objy[i];
  ans = get();
  while((double)clock() / CLOCKS_PER_SEC <= 0.9) sa();
  printf("%.0lf\n", ans);
	return 0;
}

```


还有一些习题，详见 OI-WIKI。

## 最后

感谢大家的阅读，希望能点赞评论支持一下，谢谢！