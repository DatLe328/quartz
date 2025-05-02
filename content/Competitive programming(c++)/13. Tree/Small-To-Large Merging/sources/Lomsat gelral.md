```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 1e5 + 1;
int color[MAX_N];
int64_t ans[MAX_N];
vector<int> adj[MAX_N];
map<int, int> color_count[MAX_N];
map<int, ll> max_sum[MAX_N];
int n;

void dfs(int u, int p) {
    color_count[u][color[u]]++;
    max_sum[u][1] += color[u];
    for (int v : adj[u]) {
        if (v == p) continue;
        dfs(v, u);
        if (color_count[v].size() > color_count[u].size()) {
            swap(color_count[u], color_count[v]);
            swap(max_sum[u], max_sum[v]);
        }
        for (auto [col, cnt] : color_count[v]) {
            if (color_count[u].count(col)) {
                max_sum[u][color_count[u][col]] -= col;
            }
            color_count[u][col] += cnt;
            max_sum[u][color_count[u][col]] += col;
        }
    }
    debug(color_count[u]);
    ans[u] = max_sum[u].rbegin()->second;
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> color[i];
    }
    for (int i = 1; i < n; i++) {
        int u, v;   cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    dfs(1, 1);   
    for (int i = 1; i <= n; i++) {
        cout << ans[i] << ' ';
    }

    return 0;
}
```