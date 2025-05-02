```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

void solve() {
    int n, m, h;    cin >> n >> m >> h;
    vector<vector<pair<int, int64_t>>> adj(n * 2);
    vector<bool> horse(n);
    for (int i = 0; i < h; i++) {
        int x;  cin >> x;
        x--;
        horse[x] = true;
    }
    for (int i = 0; i < m; i++) {
        int u, v, w;   cin >> u >> v >> w;
        u--, v--;
        adj[u].push_back({v, w});
        adj[u + n].push_back({v + n, w / 2});
        if (horse[u]) {
            adj[u].push_back({v + n, w / 2});
        }
        adj[v].push_back({u, w});
        adj[v + n].push_back({u + n, w / 2});
        if (horse[v]) {
            adj[v].push_back({u + n, w / 2});
        }
    }
    vector<int64_t> dist1(n * 2, LLONG_MAX), dist2(n * 2, LLONG_MAX);
    priority_queue<pair<int64_t, int>, vector<pair<int64_t, int>>, greater<pair<int64_t, int>>> pq;

    pq.push({0, 0});
    dist1[0] = 0;
    while (pq.size()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist1[u])    continue;
        for (auto [v, w] : adj[u]) {
            if (dist1[v] > dist1[u] + w) {
                dist1[v] = dist1[u] + w;
                pq.push({dist1[v], v});
            }
        }
    }
    if (dist1[n - 1] == LLONG_MAX) {
        cout << -1 << '\n';
        return;
    }
    debug(dist1);

    pq.push({0, n - 1});
    dist2[n - 1] = 0;
    while (pq.size()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist2[u])    continue;
        for (auto [v, w] : adj[u]) {
            if (dist2[v] > dist2[u] + w) {
                dist2[v] = dist2[u] + w;
                pq.push({dist2[v], v});
            }
        }
    }

    debug(dist2);
    int64_t ans = LLONG_MAX;
    for (int i = 0; i < n; i++) {
        ans = min(ans, max(min(dist1[i], dist1[i + n]), min(dist2[i], dist2[i + n])));
    }
    cout << ans << '\n';
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int tt; cin >> tt;
    while (tt--) {
        solve();
    }

    return 0;
}
```