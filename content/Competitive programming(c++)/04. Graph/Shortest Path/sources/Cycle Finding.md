```cpp
#include<bits/stdc++.h>
#define ar array
#define ll long long
using namespace std;
 
const int maxn = 3 * 1e5;
const ll INF = 1e18;
vector<ar<ll, 2>> adj[maxn];
vector<ll> dist;
int n, m, par[maxn];
 
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
 
void solve() {
	cin >> n >> m;
	for (int i = 1; i <= m; i++)
		par[i] = i;
	for (int i = 0; i < m; i++) {
		int u, v, w;	cin >> u >> v >> w;
		adj[u].push_back({v, w});
	}
	bellman_fold(1);
	cycle_detect();
}
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);
	solve();
	return 0;
}

```