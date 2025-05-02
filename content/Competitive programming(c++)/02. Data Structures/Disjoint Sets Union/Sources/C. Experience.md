```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 2e5 + 1;
int par[MAX_N], point[MAX_N], delta[MAX_N];
vector<int> child[MAX_N];
int find(int u) {
	return par[u] == u ? u : par[u] = find(par[u]);
}
void unite(int u, int v) {
	u = find(u), v = find(v);
    if (u == v) {
		return;
	}
	if (child[u].size() < child[v].size()) {
		swap(u, v);
	}
	for (auto i : child[v]) {
		point[i] += delta[v] - delta[u];
	}
	delta[v] = 0;
	child[u].insert(child[u].end(), child[v].begin(), child[v].end());
	par[v] = u;
}
void add(int x, int v) {
	x = find(x);
	delta[x] += v;
}
int query(int u) {
	int x = find(u);
	return point[u] + delta[x];
}
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	int n, q;	cin >> n >> q;
	for (int i = 1; i <= n; i++) {
		par[i] = i;
		child[i].push_back(i);
	}
	while (q--) {
		string t;	cin >> t;
		if (t == "add") {
			int x, v;	cin >> x >> v;
			add(x, v);
		}
		else if (t == "join") {
			int x, y;	cin >> x >> y;
			unite(x, y);
		}
		else {
			int x;	cin >> x;
			cout << query(x) << '\n';
		}
	}

	return 0;
}
```