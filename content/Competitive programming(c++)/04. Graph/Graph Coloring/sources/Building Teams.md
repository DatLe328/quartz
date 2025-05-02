```cpp
#include<bits/stdc++.h>
#define ll long long
#define ar array
using namespace std;
 
const int maxn = 1e5 + 1;
int n, m, color[maxn];
vector<int> adj[maxn];
 
void bfs(int s) {
	
	queue<int> q;
	color[s] = 1;
	q.push(s);
	while (q.size()) {
		int u = q.front();
		q.pop();
		for (int v : adj[u])
			if (!color[v]) {
				color[v] = (color[u] % 2) + 1;
				q.push(v);
			}
			else if (color[v] == color[u]) {
				cout << "IMPOSSIBLE";
				exit(0);
			}
	}
}
 
void solve() {
	cin >> n >> m;
	
	for (int i = 0; i < m; i++) {
		int u, v;	cin >> u >> v;
		adj[u].push_back(v);
		adj[v].push_back(u);
	}
	for (int i = 1; i <= n; i++)
		if (!color[i])
			bfs(i);
	for (int i = 1; i <= n; i++)
		cout << color[i] << ' ';
}
 
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);	
	solve();
	return 0;
}

```