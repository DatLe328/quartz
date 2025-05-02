```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

const int MAX_N = 2e5 + 1;

int n, k;
vector<int> adj[MAX_N];
int child[MAX_N];
bool vis[MAX_N];
ll ans;

void dfs(int u, int p) {
    child[u] = 1;
    for (int v : adj[u]) {
        if (v == p || vis[v]) continue;
        dfs(v, u);
        child[u] += child[v];
    }
}

int centroid(int u, int p, int sz) {
    for (int v : adj[u]) {
        if (v == p || vis[v]) continue;
        if (child[v] * 2 > sz) {
            return centroid(v, u, sz);
        }
    }
    return u;
}


int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n;
    for (int i = 1; i < n; i++) {
        int u, v; cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    dfs(1, 1);
    cout << centroid(1, 0, child[1]);
    return 0;
}

```