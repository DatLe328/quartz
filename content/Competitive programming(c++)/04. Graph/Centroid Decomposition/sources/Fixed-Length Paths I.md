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

void add(int u, int p, int depth, vector<int>& cnt) {
    if (depth > k) return;
    ans += cnt[k - depth]; // Đếm số đường đi thỏa mãn
    for (int v : adj[u]) {
        if (v == p || vis[v]) continue;
        add(v, u, depth + 1, cnt);
    }
}

void update(int u, int p, int depth, vector<int>& cnt) {
    if (depth > k) return;
    cnt[depth]++;
    for (int v : adj[u]) {
        if (v == p || vis[v]) continue;
        update(v, u, depth + 1, cnt);
    }
}

void decompose(int u) {
    dfs(u, u);
    int sz = child[u];
    if (k > sz) return;
    u = centroid(u, u, sz);
    vis[u] = true;

    vector<int> cnt(k + 1, 0);
    cnt[0] = 1;

    for (int v : adj[u]) {
        if (vis[v]) continue;
        add(v, u, 1, cnt); // Tính số đường đi từ các nhánh
        update(v, u, 1, cnt); // Cập nhật số đỉnh
    }

    for (int v : adj[u]) {
        if (!vis[v]) decompose(v);
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> k;
    for (int i = 1; i < n; i++) {
        int u, v; cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    decompose(1);
    cout << ans << '\n';

    return 0;
}

```