 #搜索 

今天心情好，发一波题解。

### 如何拿 $50\%$ 的分数？

暴力出奇迹，打表进省一！！

四重循环，如果 `grid[i][j] == 0`，那么再套二重循环，再者，如果 `grid[k][l] == 1`，那么更新最小距离，内部的二重循环跑完更新最大距离。代码如下：

```cpp
class Solution {
public:
    int maxDistance(vector<vector<int>>& grid) {
       int ans = 0;
       int land = 0;
       int ocean = 0;
       for (int i = 0; i < grid.size(); i++)
       {
           for (int j = 0; j < grid.size(); j++)
           {
               if (grid[i][j] == 0)
                {
                    int mindis = 1145141919;
                    for (int k = 0; k < grid.size(); k++)
                    {
                        for (int l = 0; l < grid.size(); l++)
                        {
                            if (grid[k][l])
                            mindis = min(mindis, abs(k - i) + abs(l - j));
                        }
                    }
                    if (mindis)
                        ans = max(ans, mindis);
                }
           }
       } 
        if (ans == 0 || ans == 1145141919)
        {
            return -1;
        }
        return ans;
    }

    
};
```

### 如何拿大部分的分数？

如果 `grid[i][j] == 0`，那么跑一遍 `bfs`，直到找到一个 `(x,y)`，使得 `grid[x][y] == 1`。

代码如下：

```cpp
class Solution {
private:
    bool vis[105][105];
    int bfs(int x, int y, vector<vector<int>>& grid)
    {
        memset(vis, 0, sizeof(vis));
        queue<pair<int, int>> q;
        q.push({x, y});
        vis[x][y] = 1;
        int nex[4][2] = {{1,0},{0,1},{-1,0},{0,-1}};
        while (q.size())
        {
            pair<int,int> data = q.front();
            q.pop();
            // cout << data.first << " " << data.second << " " << x  << " " << y << " " << grid[data.first][data.second] << endl;
            if (grid[data.first][data.second])
            {
                return abs(data.first - x) + abs(data.second - y);
            }    
            for (int i = 0; i < 4; i++)
            {
                int dx = nex[i][0] + data.first;
                int dy = nex[i][1] + data.second;
                if (dx < 0 || dy >= grid.size() || dx >= grid.size() || dy < 0 || vis[dx][dy])
                {
                    continue;
                }
                q.push({dx, dy});
                vis[dx][dy] = 1;
                
            }
        }
        return -1;
    }
public:
    int maxDistance(vector<vector<int>>& grid) {
        int ans = -1;
        for (int i = 0; i < grid.size(); i++)
        {
            for (int j = 0; j < grid.size(); j++)
            {
                if (grid[i][j] == 0)
                {
                    int ind = bfs(i, j, grid);
                    cout << i <<' ' << j << ' ' << ind << endl;
                    ans = max(ans, ind);
                }
            }
        }
        return ans;
    }

    
};
```

### 如何 AC

打多源 `bfs`优化，如果一个点已经被记录了，就不会再次被记录了。也就是 `map[i][j].step = front.step + 1`（因为偏移量与源点的曼哈顿距离为 $1$。

```cpp
class Solution {
public:
    struct node
    {
        int x, y;
        int step;
    }map[105][105], h;
    queue<node> q;
    int cnt1 = 0, cnt2 = 0;
    int maxDistance(vector<vector<int>>& grid) {
        int n = grid.size();
        for (int i = 0; i < grid.size(); i++)
        {
            for (int j = 0; j < grid.size(); j++)
            {
                map[i][j].step = grid[i][j];
                map[i][j].x = i;
                map[i][j].y = j;
                if (map[i][j].step == 1)
                {
                    cnt1++;
                }
                else if (map[i][j].step == 0)
                {
                    cnt2++;
                }
            }
        }
        if (cnt1 == n * n || cnt2 == n * n)
        {
            return -1;
        }
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                if (map[i][j].step)
                {
                    q.push(map[i][j]);
                }
            }
        }
        int nex[4][2] = {{1,0},{0,1},{-1,0},{0,-1}};
        while (q.size())
        {
            h = q.front();
            q.pop();
            for (int k = 0; k < 4; k++)
            {
                int dx = h.x + nex[k][0];
                int dy = h.y + nex[k][1];
                if (dx < 0 || dy < 0 || dx >= n || dy >= n)
                {
                    continue;
                }
                if (!map[dx][dy].step)
                {
                    map[dx][dy].step = h.step + 1;
                    q.push(map[dx][dy]);
                }
            }
        }
        return h.step - 1;
    }

    
};
```