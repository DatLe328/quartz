```cpp
void bellman_fold(int s) {
	dist.assign(n + 1, INF);
	dist[s] = 0;
	for (int i = 0; i < n - 1; i++)
		for (int u = 1; u <= n; u++)
			for (auto [v, w] : adj[u])
				if (dist[v] > dist[u] + w) {
					dist[v] = dist[u] + w;
					par[v] = u;
				}
}
 
void cycle_detect() {
	int cycle = 0;
	for (int u = 1; u <= n; u++)
		for (auto [v, w] : adj[u])
			if (dist[v] > dist[u] + w) {
				cycle = v;
				break;
			}
	if (!cycle) {
		cout << "NO";
		return;
	}
	for (int i = 0; i < n; i++) {
		cycle = par[cycle];
	}
	cout << "YES\n";
	vector<int> path;
 
	path.push_back(cycle);
	for (int i = par[cycle]; i != cycle; i = par[i]) {
		path.push_back(i);
	}
	cout << cycle << ' ';
	reverse(path.begin(), path.end());
	for (int i : path)
		cout << i << ' ';
}

```