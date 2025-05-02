```cpp
#include<bits/stdc++.h>
#define ll long long
#define ar array
using namespace std;
 
const int maxn = 1e5 + 1;
vector<ll> dist;
vector<ar<ll, 2>> adj[maxn];
int n, m, par[maxn];
 
void dijkstra(int s) {
	for (int i = 1; i <= n; i++)
		par[i] = i;
	dist.assign(n + 1, 1e18);
	dist[s] = 1;
	priority_queue<ar<ll, 2>, vector<ar<ll, 2>>, greater<ar<ll, 2>>> pq;
	pq.push({0, s});
	while (pq.size()) {
		auto [d, u] = pq.top();	pq.pop();
		if (dist[u] < d)	continue;
		for (auto [v, w] : adj[u])
			if (dist[v] > dist[u] + w) {
				par[v] = u;
				dist[v] = dist[u] + w;
				pq.push({dist[v], v});
			}
	}
}
void solve() {
	cin >> n >> m;
	for (int i = 0; i < m; i++) {
		int u, v;	cin >> u >> v;
		adj[u].push_back({v, 1});
		adj[v].push_back({u, 1});
	}
	dijkstra(1);
	if (dist[n] == 1e18)
		cout << "IMPOSSIBLE";
	else {
		vector<int> path;
		cout << dist[n] << '\n';
		while (par[n] != n) {
			path.push_back(n);
			n = par[n];
		}
		path.push_back(1);
		for (auto i = path.rbegin(); i != path.rend(); i = next(i))
			cout << *i << ' ';
	}
}
 
int main() {
	solve();
	return 0;
}

```