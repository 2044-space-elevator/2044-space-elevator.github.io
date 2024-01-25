#图论 #强连通分量 #Tarjan 
概念：
- 一个强连通分量：分量里任意两点可互相到达。
- 强连通分量个数：有多少个极大的强连通分量

强连通分量有三种常见算法：Tarjan, Kosaraju 和 Garbow。本篇文章很详细的讲解了所有三种算法。毕竟我发现洛谷题解大部分是 tarjan，两篇 Kosaraju，没有 Garbow。

剩下的懒得解释，照搬尼谷原题：

## 例题

### 题目描述

给定一张 $n$ 个点 $m$ 条边的有向图，求出其所有的强连通分量。

**注意，本题可能存在重边和自环。**

### 输入格式

第一行两个正整数 $n$ ， $m$ ，表示图的点数和边数。

接下来 $m$ 行，每行两个正整数 $u$ 和 $v$ 表示一条边。

### 输出格式

第一行一个整数表示这张图的强连通分量数目。

接下来每行输出一个强连通分量。第一行输出 1 号点所在强连通分量，第二行输出 2 号点所在强连通分量，若已被输出，则改为输出 3 号点所在强连通分量，以此类推。每个强连通分量按节点编号大小输出。

### 样例 #1

#### 样例输入 #1

```
6 8
1 2
1 5
2 6
5 6
6 1
5 3
6 4
3 4
```

#### 样例输出 #1

```
3
1 2 5 6
3
4
```

### 提示

对于所有数据，$1 \le n \le 10000$，$1 \le m \le 100000$。

## 题目要点

1. 注意要输出连通分量个数，连通分量中有输出过的结点不能再输出了。
2. 连通分量内部要排序。

## Tarjan算法

最常见也最实用的算法，不仅是强连通分量。很多算法都会依赖于 Tarjan。

Tarjan 基于 dfs，即深度优先搜索。每个强连通分量为搜索树的子树（废话）。

主要来说，Tarjan 基于一个观察，同处于一个分量中的结点必然构成搜索数的子树。找 SCC 就先得找到它在 DFS 树上的根。

用 `dfn[u]` 表示搜索时到达顶点 $u$ 的顺序编号，`low[u]` 表示以 $u$ 为根节点的 dfs 树中顺序编号最小的顶点的顺序编号，当 `dfn[u] = low[u]` 时，这颗子树就构成了 SCC。

现将顶点 $u$ 入栈，然后给 `dfn` 以及 `low` 顺序编号：

```cpp
dfn[u] = low[u] = ++dfncnt;
```

扫描 $u$ 能到达的顶点 $v$，若 $v$ 尚未访问，递归访问 $v$，并且更新 `low[u]` 为 $\min\{\rm{low}_u,\rm {low}_v\}$，这样做的目的显而易见。如果 $v$ 不属于任何一个 SCC，则设 `low[u]` 为 $\min\{\rm{low}_u,\rm {dfn}_v\}$。扫描完 $v$ 后，若 `dfn[u] == low[u]`，将 $u$ 以上所有结点出栈（已确认这些结点属于这个 SCC），而且可以确定多了一个连通分量。

代码如下：

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
const int N = 1e4 + 5, M = 1e5 + 5;
int n, m, head[N], next[N], cnt, st[N], top, dfncnt, sccsum, scc[M], dfn[N], low[N];
bool vis[N];
struct edge {
  int u, v, next;
}E[M];
void add(int u, int v) {
  E[++cnt] = {u, v, head[u]};
  head[u] = cnt; 
}

vector<int> res[N];

void tarjan(int x) {
  st[++top] = x; // 手写栈
  dfn[x] = low[x] = ++dfncnt; // 初始化
  for (int i = head[x]; i; i = E[i].next) {
    int v = E[i].v;
    if (!dfn[v]) {
      tarjan(v);
      low[x] = min(low[x], low[v]);
    } else if (!scc[v]) {
      low[x] = min(low[x], dfn[v]);
    }
  }
  if (dfn[x] == low[x]) {
    sccsum++;
    while (st[top + 1] != x) {
      int v = st[top];
      scc[v] = sccsum; // 连通分量归属
      res[sccsum].push_back(v); // 添加连通分量
      top--; // 出列！
    }
  }
}

void input() { // 朴素的输入函数
  cin >> n >> m;
  rep(i, 1, m) {
    int u, v; cin >> u >> v;
    add(u, v);
  }
}

main() {
//	int t; cin >> t; while (t--) solve();
  input();
  rep(i, 1, n) {
    if (!dfn[i]) {
      tarjan(i);
    }
  }
  cout << sccsum << endl;
  // 这里和题目要点有关
  rep(i, 1, n) {
    if (vis[scc[i]]) {
      continue;
    }
    vis[scc[i]] = 1; // 不要重复输出！！
    sort(res[scc[i]].begin(), res[scc[i]].end()); // 要排序！！
    for (int v : res[scc[i]]) {
      cout << v << ' ';
    }
    cout << endl;
  }
	return 0;
}

```
