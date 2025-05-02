```cpp
#include<bits/stdc++.h>
using namespace std;
 
#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif
 
using ll = long long;
const int MAX_N = 1e5 + 1;
const ll MOD = 1e9 + 7;
const ll INF = 1e18;
 
vector<pair<int, ll>> adj[MAX_N];
int n, m, minR[MAX_N], maxR[MAX_N], vis[MAX_N];
ll dist[MAX_N], rout[MAX_N];
 
void dijkstra(int s = 1) {
    priority_queue<pair<ll, int>, vector<pair<ll, int>>, greater<pair<ll, int>>> pq;
    pq.push({0, s});
    dist[s] = 0;
    rout[1] = 1;
    while (pq.size()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) {
            continue;
        }
        if (vis[u]) {
            continue;
        }
        vis[u] = 1;
        for (auto [v, w] : adj[u]) {
            if (dist[u] + w == dist[v]) {
                rout[v] = (rout[v] + rout[u]) % MOD;
                minR[v] = min(minR[u] + 1, minR[v]);
                maxR[v] = max(maxR[u] + 1, maxR[v]);
            }
            else if(dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                rout[v] = rout[u];
                minR[v] = minR[u] + 1;
                maxR[v] = maxR[u] + 1;
                pq.push({dist[v], v});
            }
        }
    }
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);
 
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        dist[i] = INF;
    }
    for (int i = 0; i < m; i++) {
        ll u, v, w; cin >> u >> v >> w;
        adj[u].push_back({v, w});
    }
    dijkstra();
    cout << dist[n] << ' ' << rout[n] << ' ' << minR[n] << ' ' << maxR[n];
 
    return 0;
}

```