```cpp
#include<bits/stdc++.h>
#include<vector>
#define ll long long
using namespace std;
 
const int MAX_N = 2e5 + 1;
const ll INF = 1e18;
vector<pair<ll, int>> adj[MAX_N];
vector<ll> dist;
int m, n;
void dijkstra(int s = 1) {
    dist.assign(m + 1, INF);
    dist[s] = 0;
    priority_queue<pair<ll, int>, vector<pair<ll, int>>, greater<pair<ll, int>>> pq;
    pq.push({0, s});
    while (pq.size()) {
	auto [d, u] = pq.top();	pq.pop();
	if (d > dist[u])    continue;
	for (auto [v, w] : adj[u]) {
	    if (dist[v] > dist[u] + w) {
		dist[v] = dist[u] + w;
		pq.push({dist[v], v});
	    }
	}
    }
}
void solve() {
    cin >> m >> n;
    for (int i = 0; i < n; i++) {
	ll u, v, w;	cin >> u >> v >> w;
	adj[u].push_back({v, w});
    }
    dijkstra();
    for (int i = 1; i <= m; i++)
	cout << dist[i] << ' ';
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);
    solve();
    return 0;
}

```