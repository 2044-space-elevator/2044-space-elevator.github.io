#图论 #强连通分量 #Tarjan 
概念：
- 一个强连通分量：分量里任意两点可互相到达。
- 强连通分量个数：有多少个极大的强连通分量。

强连通分量有三种常见算法：Tarjan, Kosaraju 和 Garbow。本篇文章很详细的讲解了所有三种算法。我发现洛谷题解大部分是 tarjan，两篇 Kosaraju，没有 Garbow。

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

### 过程

主要来说，Tarjan 基于一个观察，同处于一个分量中的结点必然构成搜索数的子树。找 SCC 就先得找到它在 DFS 树上的根。

用 `dfn[u]` 表示搜索时到达顶点 $u$ 的顺序编号，`low[u]` 表示以 $u$ 为根节点的 dfs 树中顺序编号最小的顶点的顺序编号，当 `dfn[u] = low[u]` 时，这颗子树就构成了 SCC。

现将顶点 $u$ 入栈，然后给 `dfn` 以及 `low` 顺序编号：

```cpp
dfn[u] = low[u] = ++dfncnt;
```

扫描 $u$ 能到达的顶点 $v$，若 $v$ 尚未访问，递归访问 $v$，并且更新 `low[u]` 为 $\min\{\rm{low}_u,\rm {low}_v\}$，这样做的目的显而易见。如果 $v$ 不属于任何一个 SCC，则设 `low[u]` 为 $\min\{\rm{low}_u,\rm {dfn}_v\}$。扫描完 $v$ 后，若 `dfn[u] == low[u]`，将 $u$ 以上所有结点出栈（已确认这些结点属于这个 SCC），而且可以确定多了一个连通分量。

复杂度 $\mathcal{O} (n+m)$。

### 代码

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

## Garbow 算法

这个算法比较罕见，比如洛谷题解区里还没有一篇讲 Garbow 的。所以我也花了较久的时间到 OI-Wiki 上自己琢磨，~~因为没有洛谷题解了。~~

**Garbow 是 Tarjan 的另一种实现。** 复杂度和 Tarjan 一样，**注意不是优化！** 说 Garbow 是 Tarjan 优化的小伙伴给自己一巴掌。

### 过程

Tarjan 用 `dfn` 与 `low` 计算 SCC 的根，Garbow 维护两个栈栈，第二个栈用来确定何时从第一个栈 pop 属于同一个强连通分量的结点。从节点 $u$ 开始 DFS 的过程中，当一个路径显示这组结点属于同一个 SCC 时，只要栈顶节点的访问时间大于根节点 $u$ 的访问时间，就从第二个栈中弹出这个结点，最后只留下结点 $u$。此过程中**每一个被弹出的结点都属于一个 SCC。**

回溯到 $u$ 时，若这个节点在第二个栈的顶部，说明此节点是 SCC 的起始节点，此节点后搜索到的这些结点都属于同个 SCC，所以从第一个栈中弹出那些结点，构成 SCC。

发现了没？Garbow 和 Tarjan 有异曲同工之妙，都是基于计算 SCC 的根的，但一个用 `dfn` 数组，一个用栈。

### 代码

> 配了很详细的注释了。是完整可 AC 代码哈。

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

const int N = 1e4 + 10, M = 1e5 + 10;
int n, m, low[N], scc[N], st1[N], st2[N], head[N], cnt, st_size1, st_size2, sccsum;
/**
 * low                进入顶点时间
 * scc                所属 scc
 * st1/st2            所属堆栈
 * head               存图的辅助数组，记录该点下一条出边
 * cnt                图的边数
 * st_size1/st_size2  栈的长度
 * sccsum             SCC个数
*/
bool output_vis[N];
struct edge {
  int u, v, next;
}E[M];
vct<int> res[N];

void add(int u, int v) {
  E[++cnt] = {u, v, head[u]};
  head[u] = cnt;
}

void input() {
  cin >> n >> m;
  rep(i, 1, m) {
    int u, v;
    cin >> u >> v;
    add(u, v);
  }
  cnt = 0;
  // cnt 清空，garbow函数进行回收（
}

void garbow(int u) {
  st1[++st_size1] = u;
  st2[++st_size2] = u;
  low[u] = ++cnt;
  for (int i = head[u]; i; i = E[i].next) {
    int v = E[i].v;
    // 老套路
    if (!low[v]) {
      garbow(v);
    }
    else if (!scc[v]) {
      while (low[st2[st_size2]] > low[v]) {
        st_size2--;
      }
    }
  }
  if (st2[st_size2] == u) {
    st_size2--;
    sccsum++;
    do {
      scc[st1[st_size1]] = sccsum;
    }while (st1[st_size1--] != u);
  }
}


main() {
//	int t; cin >> t; while (t--) solve();
  input();
  rep(i, 1, n) {
    if (!low[i]) {
      garbow(i);
    }
  }
  cout << sccsum << endl;
  rep(i, 1, n) {
    res[scc[i]].push_back(i);
  }
  rep(i, 1, n) {
    sort(res[i].begin(), res[i].end());
  }
  rep(i, 1, n) {
    if (!output_vis[scc[i]]) {
      for (int v : res[scc[i]]) {
        cout << v << ' ';
      }
      output_vis[scc[i]] = 1;
      cout << endl;
    }
  }
	return 0;
}

```

## Kosaraju 算法

### 故事

很有意思的是，和爱迪生的灯泡一样。Kosaraju 算法不是由 Kosaraju 直接发明的（滑稽）。S. Rao Kosaraju 在 1978 年一篇未发表的论文中提出，但 Micha Sharir 最早发表了它。

### 预处理

对于图 $G$ 的每一条边 $(u,v)$，将边 $(v,u)$ 加入图 $G'$ 中。即创建反图。

### 过程

此算法依靠两次简单的 dfs：

- 第一次 dfs，选取任意结点作为起点，遍历所有未访问过的顶点，并在回溯之前给顶点编号（即访问顺序），也就是传说中的后序遍历。

- 第二次 dfs，对于反向后的图，以标号最大的顶点作为起点开始 BFS。然后遍历到的点的集合就是一个 SCC。而对于所有未访问过的结点，再选取最大的做重复。

结束后所有 SCC 也就出来啦，复杂度 $\mathcal {O}(n+m)$。

### 代码

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

const int N = 1e4 + 10, M = 1e5 + 10;
int n, m, cnt, head[N], color[N], cnt2, head2[N], tot;
vct<int> col[N];
bool vis[N];
struct edge {
  int u, v, next;
}E[M], E2[M];
stack<int> st;
void add(int u, int v, int type) {
  if (type == 1) {
    E[++cnt] = {u, v, head[u]};
    head[u] = cnt;
  } else {
    E2[++cnt2] = {u, v, head2[u]};
    head2[u] = cnt2;
  }
}

void dfs1(int u) {
  vis[u]= 1;
  for (int i = head[u]; i; i = E[i].next) {
    int v = E[i].v;
    if (!vis[v]) {
      dfs1(v);
    }
  }
  st.push(u);
}

void dfs2(int u) {
  col[tot].push_back(u); color[u] = tot; vis[u] = 1;
  for (int i = head2[u]; i; i = E2[i].next) {
    int v = E2[i].v;
    if (!vis[v]) {
      dfs2(v);
    }
  }
}

void input() {
  cin >> n >> m;
  rep(i, 1, m) {
    int u, v; cin >> u >> v;
    add(u, v, 1); add(v, u, 2);
  }
}


main() {
//	int t; cin >> t; while (t--) solve();
  input();
  rep(i, 1, n) {
    if (!vis[i]) {
      dfs1(i);
    }
  }
  memset(vis, 0, sizeof(vis));
  while (st.size()) {
    int frt = st.top(); st.pop();
    if (!vis[frt]) {
      ++tot; dfs2(frt);
    }
  }
  rep(i, 1, tot) {
    sort(col[i].begin(), col[i].end());
  }
  cout << tot << endl;
  memset(vis, 0, sizeof(vis));
  rep(i, 1, n) {
    if (!vis[color[i]]) {
      for (int v : col[color[i]]) {
        cout << v << ' ';
      }
      cout << endl;
      vis[color[i]] = 1;
    }
  }
	return 0;
}

```

## 参考文献

- [OI-Wiki 强连通分量](https://oi-wiki.org/graph/scc/)
- [Fido_Puppy 的洛谷题解](https://www.luogu.com.cn/problem/solution/B3609)
- [Daidly 的洛谷题解](https://www.luogu.com.cn/blog/271736/solution-b3609)