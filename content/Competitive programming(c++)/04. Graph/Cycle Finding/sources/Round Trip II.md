```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 1e5 + 1;
int vis[MAX_N], par[MAX_N], s, e, n, m;
vector<int> adj[MAX_N];

void dfs(int u, int p) {
	vis[u] = 1;
	par[u] = p;
	for (int v : adj[u]) {
		if (!vis[v]) {
			dfs(v, u);
			if (s != -1) {
				return;
			}
		}
		else if (vis[v] == 1) {
			s = v;
			e = u;
			return;
		}
	}
	vis[u] = 2;
}
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	cin >> n >> m;
	for (int i = 0; i < m; i++) {
		int u, v;	cin >> u >> v;
		adj[u].push_back(v);
	}
	s = -1;
	for (int i = 1; i <= n; i++) {
		if (!vis[i]) {
			dfs(i, i);
			if (s != -1) break;
		}
	}
	if (s == -1) {
		cout << "IMPOSSIBLE\n";
	}
	else {
		vector<int> trace;
		trace.push_back(s);
		while (e != s) {
			trace.push_back(e);
			e = par[e];
		}
		trace.push_back(s);
		reverse(trace.begin(), trace.end());
		cout << trace.size() << '\n';
		for (int i : trace) {
			cout << i << ' ';
		}
	}

	return 0;
}
```