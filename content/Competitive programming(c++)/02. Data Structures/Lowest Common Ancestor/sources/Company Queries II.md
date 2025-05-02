```cpp
#include<bits/stdc++.h>
using namespace std;
const int MAX_N = 2e5 + 1;
const int MAX_L = 20;
 
vector<int> adj[MAX_N];
int par[MAX_N][MAX_L], dept[MAX_N];
 
void dfs(int u, int p = 0) {
	par[u][0] = p;
	for (int v = 1; v < MAX_L; v++)
		par[u][v] = par[par[u][v - 1]][v - 1];
	for (int v : adj[u]) {
		if (v == p)	continue;
		dept[v] = dept[u] + 1;
		dfs(v, u);
	}
}
 
int ancestor(int u, int k) {
	for (int i = 0; i < MAX_L; i++) {
		if (k & (1 << i))
			u = par[u][i];
	}
	return u;
}
 
int lca(int u, int v) {
	if (dept[u] < dept[v])
		swap(u, v);
	u = ancestor(u, dept[u] - dept[v]);
	if (u == v)
		return u;
	for (int i = MAX_L - 1; i >= 0; i--) {
		if (par[u][i] != par[v][i])
			u = par[u][i], v = par[v][i];
	}
	return par[u][0];
}
 
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);
 
	int n, q;	cin >> n >> q;
	for (int u = 2; u <= n; u++) {
		int v;	cin >> v;
		adj[u].push_back(v);
		adj[v].push_back(u);
	}
	dfs(1);
	while (q--) {
		int u, v;	cin >> u >> v;
		int que = lca(u, v);
		cout << que << '\n';
	}
 
	return 0;
}
```