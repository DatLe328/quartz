```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 1e5 + 1;
int num[MAX_N], low[MAX_N], bridge = 0, timer = 0, n, m;
int joint[MAX_N];
vector<int> adj[MAX_N];

void dfs(int u, int p) {
    num[u] = low[u] = ++timer;
    int child = 0;
    for (int v : adj[u]) {
        if (v == p) {
            continue;
        }
        if (!num[v]) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);
            child++;
            if (num[v] == low[v]) {
                bridge++;
            }
            if (u == p) {
                if (child > 1) {
                    joint[u] = 1;
                }
            }
            else if (low[v] >= num[u]) {
                joint[u] = 1;
            }
        }
        else {
            low[u] = min(low[u], num[v]);
        }
    }
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        int u, v;   cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    for (int i = 1; i <= n; i++) {
        if (!num[i]) {
            dfs(i, i);
        }
    }
    int cntJoint = 0;
    for (int i = 1; i <= n; i++) {
        cntJoint += joint[i];
    }
    cout << cntJoint << ' ' << bridge;

    return 0;
}

```