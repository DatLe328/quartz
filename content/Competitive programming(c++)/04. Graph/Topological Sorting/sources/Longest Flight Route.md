```cpp
#include<bits/stdc++.h>
using namespace std;
 
#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif
 
const int MAX_N = 2e5 + 1;
vector<int> adj[MAX_N];
int n, m, vis[MAX_N], dp[MAX_N], par[MAX_N];
vector<int> topo;
void dfs(int u) {
    vis[u] = 1;
    for (int v: adj[u]) {
        if (!vis[v]) {
            dfs(v);
        }
    }
    topo.push_back(u);
}
 
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);
 
    cin >> n >> m;
    for (int i = 1; i <= m; i++) {
        int u, v;   cin >> u >> v;
        adj[u].push_back(v);
    }
    dp[1] = 1;
    par[1] = -1;
    dfs(1);
    reverse(topo.begin(), topo.end());
    for (int u : topo) {
        for (int v : adj[u]) {
            if (dp[v] < dp[u] + 1) {
                dp[v] = dp[u] + 1;
                par[v] = u;
            }
        }
    }
    if (dp[n] == 0) {
        cout << "IMPOSSIBLE\n";
    }
    else {
        vector<int> trace{n};
        while (par[n] != -1) {
            trace.push_back(par[n]);
            n = par[n];
        }
        cout << trace.size() << '\n';
        reverse(trace.begin(), trace.end());
        for (int i : trace) {
            cout << i << ' ';
        }
    }
    return 0;
}

```