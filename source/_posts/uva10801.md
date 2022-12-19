---
title: UVA 10801 Lift Hopping
tags: [Algorithm]
categories: [Algorithm]
date: 2022/12/19 09:46:25
---

## Intuition

如果直接建圖會變成multiple source, 不符合dijkstra，所以將sources和destinations都連上node 0 和 node 700，再使用dijkstra演算法，尋找0到700最短路徑。

## Code

```cpp
#include <iostream>
#include <climits>
#include <algorithm>
#include <vector>
#include <string>
#include <cstring>
#include <sstream>
#include <queue>
using namespace std;

#define mp make_pair
#define pr pair<int,int>
#define INF 9999999

vector<vector<pair<int, int>>> edges;
vector<int> T;
priority_queue<pr, vector<pr>, greater<pr>>pq;
vector<int> dis;

void dijkstra() {
    dis.assign(701, INF);
    dis[0] = 0;
    pq.emplace(mp(0, 0));
    while (!pq.empty()) {
        int x = pq.top().second;
        int val = pq.top().first;
        pq.pop();
        for (auto y : edges[x]) {
            //cout << y.first << " ";
            if (dis[y.first] > dis[x] + y.second) {
                dis[y.first] = dis[x] + y.second;
                pq.push(mp(dis[y.first], y.first));
            }
        }//cout << endl;
    }

}

void connect(int x, int y, int cost) {
    edges[x].emplace_back(mp(y, cost));
    edges[y].emplace_back(mp(x, cost));
}

void print(int x) {
    for (auto it : edges[x]) {
        cout << it.first << " " << it.second << endl;
    }
}

int main() {
    int n, k;
    while (cin >> n >> k) {
        T.assign(n + 1, 0);
        edges.assign(701, vector<pair<int, int>>());
        
        for (int i = 1; i <= n; i++)
            cin >> T[i];
        cin.ignore();
        // connect linearly
        for (int i = 1; i <= n; i++) {
            string s;
            getline(cin, s);
            stringstream ss(s);
            int x, y;
            ss >> x;
            if (x == 0)
                connect(0, 100 * i, 0);
            while (ss >> y) {
                connect(100 * i + x, 100 * i + y, (y - x) * T[i]);
                x = y;
            }
        }
        
        // connect different -elevator
        for (int i = 0; i < 100; i++)
            for (int x = 1; x <= n; x++)
                for (int y = x + 1; y <= n; y++)
                    if (!edges[x * 100 + i].empty() && !edges[y * 100 + i].empty())
                        connect(x * 100 + i, y * 100 + i, 60);
        // connect endpoint
        for (int x = 1; x <= n; x++)
            if (!edges[x * 100 + k].empty())
                connect(x * 100 + k, 700, 0);
        //print(230);
        dijkstra();
        if (dis[700] == INF)
            cout << "IMPOSSIBLE\n";
        else cout << dis[700] << endl;
    }
}

```
