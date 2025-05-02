```cpp
struct DSU {
	vector<int> par, sz, point, delta;
	DSU(int n) : par(n, - 1), sz(n, 1), point(n, 0), delta(n, 0) {}
	int find(int u) {
		return par[u] == -1 ? u : par[u] = find(par[u]);
	}
	void unite(int u, int v) {
		u = find(u);
		v = find(v);
		if (u == v) {
			return;
		}
		if (sz[u] < sz[v]) {
			swap(u, v);
		}
		sz[u] += sz[v];
		par[v] = u;
	}	
};
```