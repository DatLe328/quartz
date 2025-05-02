```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 1e5 + 1;
const int MAX_L = 20;

int par[MAX_N][MAX_L], depth[MAX_N], deg[MAX_N], edg_cnt[MAX_N];
vector<int> adj[MAX_N], edg_id[MAX_N];

void dfs(int u, int p) {
    par[u][0] = p;
    for (int i = 1; i < MAX_L; i++) {
        par[u][i] = par[par[u][i - 1]][i - 1];
    }
    for (int v : adj[u]) {
        if (v == p) continue;
        depth[v] = depth[u] + 1;
        dfs(v, u);
    }
}
int ancestor(int u, int k) {
    for (int i = 0; i < MAX_L; i++) {
        if (k & (1 << i)) {
            u = par[u][i];
        }
    }
    return u;
}
int lca(int u, int v) {
    if (depth[u] < depth[v]) swap(u, v);
    u = ancestor(u, depth[u] - depth[v]);
    if (u == v) return u;
    for (int i = MAX_L - 1; i >= 0; i--) {
        if (par[u][i] != par[v][i]) {
            u = par[u][i], v = par[v][i];
        }
    }
    return par[u][0];
}

void query(int u, int v) {
    int root = lca(u, v);
    deg[u]++;
    deg[v]++;
    deg[root] -= 2;
}

void solve(int u, int p) {
    for (int i = 0; i < int(adj[u].size()); i++) {
        int v = adj[u][i];
        int pos = edg_id[u][i];
        if (v == p) continue;
        solve(v, u);
        edg_cnt[pos] += deg[v];
        deg[u] += deg[v];
    }
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n;  cin >> n;
    vector<long long> c1(n + 1), c2(n + 1);
    for (int i = 1; i < n; i++) {
        int u, v;   cin >> u >> v >> c1[i] >> c2[i];
        adj[u].push_back(v);
        adj[v].push_back(u);
        edg_id[u].push_back(i);
        edg_id[v].push_back(i);
    }
    dfs(1, 1);
    for (int i = 1; i < n; i++) {
        query(i, i + 1);
    }
;
    solve(1, 1);
    long long ans = 0;
    for (int i = 1; i < n; i++) {
        ans += min(edg_cnt[i] * c1[i], c2[i]);
    }
    cout << ans;

    return 0;
}
```