```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
 
#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif
 
const int MAX_N = 2e5 + 1;
const ll MOD = 1e9 + 7;
 
vector<int> adj[MAX_N];
int n, m, vis[MAX_N];
vector<int> topo;
ll dp[MAX_N];
 
void dfs(int u) {
    vis[u] = 1;
    for (int v : adj[u]) {
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
    dfs(1);
    reverse(topo.begin(), topo.end());
    dp[1] = 1;
    for (int u : topo) {
        cerr << u << ": ";
        for (int v : adj[u]) {
            dp[v] = (dp[v] + dp[u]) % MOD;
        }
    }
    cout << dp[n];
 
    return 0;
}

```