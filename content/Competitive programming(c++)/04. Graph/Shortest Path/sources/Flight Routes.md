```cpp
#include<bits/stdc++.h>
#define ll long long
#define ar array
using namespace std;
 
const int maxn = 1e5 + 1;
int n, m, k;
vector<ar<ll, 2>> adj[maxn];
priority_queue<ll> dist[maxn];
 
void dijkstra(int s) {
	dist[s].push(0);
	priority_queue<ar<ll, 2>, vector<ar<ll, 2>>, greater<ar<ll, 2>>> pq;
	pq.push({0, s});
	while (pq.size()) {
		auto [d, u] = pq.top();	pq.pop();
		if (d > dist[u].top())	continue;
		for (auto [v, w] : adj[u]) {
			ll tmp = w + d;
			if (dist[v].size() < k) {
				dist[v].push(tmp);
				pq.push({tmp, v});
			}
			else if (dist[v].top() > tmp) {
				dist[v].pop();
				dist[v].push(tmp);
				pq.push({tmp, v});
			}
		}
	}
} 
void solve() {
	cin >> n >> m >> k;
	for (int i = 0; i < m; i++) {
		int u, v, w;	cin >> u >> v >> w;
		adj[u].push_back({v, w});
	}
	
	dijkstra(1);
	
	vector<ll> ans;
	while (dist[n].size()) {
		ans.push_back(dist[n].top());
		dist[n].pop();
	}
	reverse(ans.begin(), ans.end());
	for (ll i : ans)
		cout << i << ' ';
}
 
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);	
	solve();
	return 0;
}

```