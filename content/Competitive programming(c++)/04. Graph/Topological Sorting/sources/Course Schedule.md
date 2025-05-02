```cpp
#include<bits/stdc++.h>
using namespace std;
 
const int MAX_N = 1e5 + 1;
int n, m, vis[MAX_N];
vector<int> adj[MAX_N], topo;
 
void dfs(int u) {
    vis[u] = 1;
    for (int v : adj[u]) {
        if (!vis[v]) {
            dfs(v);
        }
        else if (vis[v] == 1) {
            cout << "IMPOSSIBLE";
            exit(0);
        }
    }
    vis[u] = 2;
    topo.push_back(u);
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
    }
    for (int i = 1; i <= n; i++) {
        if (!vis[i]) {
            dfs(i);
        }
    }
    reverse(topo.begin(), topo.end());
    for (int i : topo) {
        cout << i << ' ';
    }
 
    return 0;
}

```