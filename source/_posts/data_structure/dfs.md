---
title: DFS Algorithms
tags: [Algorithm]
categories: [Algorithm]
date: 2022/12/8 09:46:25
---

DFS, aka. depth-first search, is a nice tool to look data in depth-first manner.
an example to travel through graph will be this :

```cpp
void dfs(Node* u) {
    if u is visited:
        return;
    Visit();
    for edges(u,v) connect with node:
        dfs(v);
}
```

## Examples:

### DFS

#### UVA - 280 Vertex

```cpp
#include <bits/stdc++.h>

using namespace std;

#define ll long long
#define mp make_pair
#define ALL(x) x.begin(), x.end()

const int mod = 1e9 + 7;

vector<vector<int>> ver;
vector<int> vis;

int dfs(int x, int start = 0)
{
    if (vis[x])
        return 0;
    if (!start)
        vis[x] = 1;
    int res = 1;
    for (int i = 0; i < ver[x].size(); i++)
        res += dfs(ver[x][i]);
    return res;
}

void solve(int v)
{
    ver.clear();
    ver.resize(v + 1);
    int temp, t1;
    while (cin >> temp && temp)
    {
        while (cin >> t1 && t1)
            ver[temp].emplace_back(t1);
    }
    int m;
    cin >> m;
    for (int i = 0; i < m; i++)
    {
        vis.clear();
        vis.resize(v + 1, 0);
        cin >> temp;
        cout << v + 1 - dfs(temp, 1);
        for (int j = 1; j <= v; j++)
        {
            if (vis[j] == 0)
                cout << " " << j;
        }
        cout << "\n";
    }
}

int main(int argc, const char *argv[])
{
    int v;
    while (cin >> v && v)
        solve(v);
    return 0;
}

```

#### UVA - 614

HAVE TO USE `setw`. (very important!!!)

```cpp
//
//  main.cpp
//  template
//
//  Created by Ryan Kert on 2022/10/22.
//
#include <bits/stdc++.h>

using namespace std;

#define ll long long
#define fast                      \
    ios_base::sync_with_stdio(0); \
    cin.tie(0);                   \
    cout.tie(0)
#define mp make_pair
#define ALL(x) x.begin(), x.end()

const int mod = 1e9 + 7;

int r[6];
int m[12][12];
int vis[12][12];

void draw()
{
    cout << "+";
    for (int i = 0; i < r[1]; i++)
        cout << "---+";
    cout << "\n";
    for (int i = 0; i < r[0]; i++)
    {
        cout << "|";
        for (int j = 0; j < r[1]; j++)
        {
            if (vis[i][j] == 0)
                cout << "   ";
            if (vis[i][j] == -1)
                cout << "???";
            if (vis[i][j] > 0)
                cout << setw(3) << vis[i][j];
            if (m[i][j] % 2 == 1 || j + 1 == r[1])
                cout << "|";
            else
                cout << " ";
        }
        cout << "\n+";
        for (int j = 0; j < r[1]; j++)
        {
            if (m[i][j] > 1 || i + 1 == r[0])
                cout << "---+";
            else
                cout << "   +";
        }
        cout << "\n";
    }
    cout << "\n\n";
}

bool dfs(int x, int y, int ind) // x : row, y : col
{
    if (vis[x][y] != 0)
        return false;
    vis[x][y] = ind++;
    if (x == r[4] && y == r[5])
        return true;

    if (y > 0 && m[x][y - 1] % 2 == 0) // left
        if (dfs(x, y - 1, ind))
            return true;
    if (x > 0 && m[x - 1][y] < 2) // up
        if (dfs(x - 1, y, ind))
            return true;
    if (y + 1 < r[1] && m[x][y] % 2 == 0) // right
        if (dfs(x, y + 1, ind))
            return true;
    if (x + 1 < r[0] && m[x][y] < 2) // down
        if (dfs(x + 1, y, ind))
            return true;
    vis[x][y] = -1;
    ind--;
    return false;
}

void solve()
{
    for (int i = 0; i < r[0]; i++)
        for (int j = 0; j < r[1]; j++)
            cin >> m[i][j];
    memset(vis, 0, sizeof(vis));
    for (int i = 2; i < 6; i++)
        r[i]--;
    dfs(r[2], r[3], 1);
    draw();
}

int main(int argc, const char *argv[])
{
    fast;
    int cnt = 1;
    while (cin >> r[0] >> r[1] >> r[2] >> r[3] >> r[4] >> r[5] && r[0])
    {
        cout << "Maze " << cnt++ << "\n\n";
        solve();
    }

    return 0;
}

```

#### UVA - 12442 Forwarding Emails

brute force = TLE
Therefore, we think that if we had traveled the vertex before, we don't have to travel it again, since if it has been traversed, the traveler is definitely possessed greater reachable person.

```cpp
//
//  main.cpp
//  template
//
//  Created by Ryan Kert on 2022/10/22.
//
#include <bits/stdc++.h>

using namespace std;

#define ll long long
#define fast                      \
    ios_base::sync_with_stdio(0); \
    cin.tie(0);                   \
    cout.tie(0)
#define mp make_pair
#define ALL(x) x.begin(), x.end()

const int mod = 1e9 + 7;

int v[50001];
bool vis[50001], dfsV[50001];

int dfs(int i)
{
    if (dfsV[i])
        return 0;
    vis[i] = dfsV[i] = true;
    return 1 + dfs(v[i]);
}

void solve()
{
    int m;
    cin >> m;
    memset(v, 0, m + 1);
    for (int i = 0; i < m; i++)
    {
        int ind;
        cin >> ind;
        cin >> v[ind];
    }
    int res = 0, idx;
    memset(vis, false, m + 1);
    for (int i = 1; i <= m; i++)
    {
        if (!vis[i])
        {
            memset(dfsV, false, m + 1);
            int temp = dfs(i);
            if (res < temp)
            {
                res = temp;
                idx = i;
            }
        }
    }
    cout << idx << endl;
}

int main(int argc, const char *argv[])
{
    fast;
    int t;
    cin >> t;
    for (int i = 0; i < t; i++)
    {
        cout << "Case " << i + 1 << ": ";
        solve();
    }

    return 0;
}

```
