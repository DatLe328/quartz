```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 5e4 + 1;
vector<int> adj[MAX_N];
pair<int, int> edgs[MAX_N];
set<int> val[MAX_N];
int n, m, par[MAX_N], ans[MAX_N];

void dfs(int u, int p) {
    par[u] = p;
    for (int v : adj[u]) {
        if (v == p) continue;
        dfs(v, u);
        if (val[u].size() < val[v].size()) {
            swap(val[u], val[v]);
        }
        for (auto p : val[v]) {
            if (val[u].find(p) != val[u].end()) {
                val[u].erase(p);
            }
            else {
                val[u].insert(p);
            }
        }
    }
    ans[u] = val[u].empty() ? -1 : *val[u].begin();
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> m;
    for (int i = 1; i < n; i++) {
        int u, v;   cin >> u >> v;
        edgs[i] = {u, v};
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    for (int i = 0; i < m; i++) {
        int u, v, w;    cin >> u >> v >> w;
        val[u].insert(w);
        val[v].insert(w);
    }
    dfs(1, 1);
    for (int i = 1; i <= n; i++) {
        debug(val[i]);
    }
    for (int i = 1; i < n; i++) {
        cout << ans[par[edgs[i].first] == edgs[i].second ? edgs[i].first : edgs[i].second] << '\n';
    }

    return 0;
}
```