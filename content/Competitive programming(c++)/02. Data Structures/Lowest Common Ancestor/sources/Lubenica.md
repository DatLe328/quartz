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
struct Node {
    int par, mx, mn;
    Node() : mx(INT_MIN), mn(INT_MAX) {}
};

Node up[MAX_N][MAX_L];
int depth[MAX_N], n, m;
vector<pair<int, int>> adj[MAX_N];

void dfs(int u, int p) {
    up[u][0].par = p;
    for (auto [v, w] : adj[u]) {
        if (v == p) continue;
        depth[v] = depth[u] + 1;
        up[v][0].mx = up[v][0].mn = w;
        dfs(v, u);
    }
}
void build_lca() {
    dfs(1, 1);
    for (int i = 1; i < MAX_L; i++) {
        for (int u = 1; u <= n; u++) {
            up[u][i].par = up[up[u][i - 1].par][i - 1].par;
            up[u][i].mx = max(up[u][i - 1].mx, up[up[u][i - 1].par][i - 1].mx);
            up[u][i].mn = min(up[u][i - 1].mn, up[up[u][i - 1].par][i - 1].mn);
        }
    }
}

void lca(int u, int v) {
    Node ans;
    if (depth[u] < depth[v]) {
        swap(u, v);
    }
    int k = depth[u] - depth[v];
    for (int i = 0; i < MAX_L; i++) {
        if (k & (1 << i)) {
            ans.mx = max(ans.mx, up[u][i].mx);
            ans.mn = min(ans.mn, up[u][i].mn);
            u = up[u][i].par;
        }
    }
    if (u == v) {
        cout << ans.mn << ' ' << ans.mx << '\n';
        return;
    }
    for (int i = MAX_L - 1; i >= 0; i--) {
        if (up[u][i].par != up[v][i].par) {
            ans.mx = max({ans.mx, up[u][i].mx, up[v][i].mx});
            ans.mn = min({ans.mn, up[u][i].mn, up[v][i].mn});
            u = up[u][i].par, v = up[v][i].par;
        }
    }

    ans.mx = max({ans.mx, up[u][0].mx, up[v][0].mx});
    ans.mn = min({ans.mn, up[u][0].mn, up[v][0].mn});
    cout << ans.mn << ' ' << ans.mx << '\n';
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n;
    for (int i = 1; i < n; i++) {
        int u, v, w;   cin >> u >> v >> w;
        adj[u].push_back({v, w});
        adj[v].push_back({u, w});
    }
    build_lca();
    int q;
    cin >> q;
    while (q--) {
        int u, v;   cin >> u >> v;
        lca(u, v);
    }

    return 0;
}
```