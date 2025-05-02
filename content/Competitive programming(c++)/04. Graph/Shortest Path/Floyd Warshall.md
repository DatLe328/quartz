```cpp
void floyd_warshall() {
	for (int k = 1; k <= n; k++) 
		for (int u = 1; u <= n; u++)
			for (int v = 1; v <= n; v++)
				if (dist[u][v] > dist[u][k] + dist[k][v])
					dist[u][v] = dist[u][k] + dist[k][v];
}
 
void solve() {
	cin >> n >> m >> q;
	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= n; j++)
			dist[i][j] = (i == j) ? 0 : INF;
	for (int i = 0; i < m; i++) {
		long long u, v, w;	cin >> u >> v >> w;
		dist[u][v] = dist[v][u] = min(dist[u][v], w);
	}
	floyd_warshall();
	while (q--) {
		int u, v;	cin >> u >> v;
		if (dist[u][v] == INF)
			cout << -1 << '\n';
		else
			cout << dist[u][v] << '\n';
	}
}

```