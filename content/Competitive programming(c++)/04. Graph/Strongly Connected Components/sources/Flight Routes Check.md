```cpp
#include<bits/stdc++.h>
using namespace std;
 
const int MAX_N = 1e5 + 1;
vector<int> adj[MAX_N], r_adj[MAX_N];
int n, m, vis[MAX_N];
 
void dfs1(int u) {
    vis[u] = 1;
    for (int v : adj[u]) {
        if (!vis[v]) {
            dfs1(v);
        }
    }
}
void dfs2(int u) {
    vis[u] = 1;
    for (int v : r_adj[u]) {
        if (!vis[v]) {
            dfs2(v);
        }
    }
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);
 
    if(fopen("A_input.txt", "r")) {
        freopen("A_input.txt", "r", stdin);
    }
 
    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        int u, v;   cin >> u >> v;
        adj[u].push_back(v);
        r_adj[v].push_back(u);
    }
    dfs1(1);
    for (int i = 1; i <= n; i++) {
        if (!vis[i]) {
            cout << "NO\n";
            cout << 1 << ' ' << i << '\n';
            return 0;
        }
    }
    memset(vis, 0, sizeof vis);
    dfs2(1);
    for (int i = 1; i <= n; i++) {
        if (!vis[i]) {
            cout << "NO\n";
            cout << i << ' ' << 1 << '\n';
            return 0;
        }
    }
    cout << "YES\n";
 
    return 0;
}

```