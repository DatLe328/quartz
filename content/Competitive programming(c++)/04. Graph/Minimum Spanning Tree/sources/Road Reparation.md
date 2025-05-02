```cpp
#include<bits/stdc++.h>
#define ar array
#define ll long long
using namespace std;
const int maxn = 1e5 + 1;
int n, m, vis[maxn];
vector<ar<ll, 2>> adj[maxn];
 
void prim(int s = 1) {
	priority_queue<ar<ll, 2>, vector<ar<ll, 2>>, greater<ar<ll, 2>>> pq;
	pq.push({0, s});
	ll weight = 0;
	while (pq.size()) {
		auto [d, u] = pq.top();	pq.pop();
		if (!vis[u]) {
			vis[u] = 1;
			weight += d;
			for (auto [v, w] : adj[u])
				if (!vis[v])
					pq.push({w, v});
		}
	}
	for (int i = 1; i <= n; i++)
		if (!vis[i]) {
			cout << "IMPOSSIBLE";
			return;
		}
	cout << weight;
}
 
void solve() {
	cin >> n >> m;
 
	for (int i = 0; i < m; i++) {
		int u, v, w;	cin >> u >> v >> w;
		adj[u].push_back({v, w});
		adj[v].push_back({u, w});
	}
	prim();
}
 
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);
	solve();
	return 0;
}

```