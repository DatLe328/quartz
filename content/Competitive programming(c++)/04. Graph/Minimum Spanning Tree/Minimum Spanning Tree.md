# Prim
The main idea just like [[Dijkstra]] algorithm, for each edge push into priority_queue and take the vertex hasn't visited add to MST
```cpp
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

```