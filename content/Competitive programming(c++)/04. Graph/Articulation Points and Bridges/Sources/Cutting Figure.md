```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	int n, m;	cin >> n >> m;
	vector<string> s(n);
	vector<vector<int>> adj(n * m);
	int check = 0;
	for (auto &i : s) {
		cin >> i;
	}
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < m; j++) {
			if (s[i][j] == '.')	continue;
			check++;
			if (i + 1 < n && s[i + 1][j] == '#') {
				adj[i * m + j].push_back((i + 1) * m + j);
				adj[(i + 1) * m + j].push_back(i * m + j);
			}
			if (j + 1 < m && s[i][j + 1] == '#') {
				adj[i * m + j].push_back(i * m + j + 1);
				adj[i * m + j + 1].push_back(i * m + j);
			}
		}
	}
	if (check < 3) {
		cout << -1 << '\n';
		return 0;
	}
	vector<int> num(n * m), low(n * m), joint(n * m);
	int timer = 0;
	function<void(int, int)> dfs = [&] (int u, int p) {
		num[u] = low[u] = ++timer;
		int child = 0;
		for (int v : adj[u]) {
			if (v == p)	continue;
			if (!num[v]) {
				dfs(v, u);
				low[u] = min(low[u], low[v]);
				child++;
				if (u == p) {
                if (child > 1) {
                    joint[u] = 1;
                }
				}
				else if (low[v] >= num[u]) {
					joint[u] = 1;
				}
			}
			else {
				low[u] = min(low[u], num[v]);
			}
		}
	};
	for (int i = 0; i < n * m; i++) {
		if (!num[i]) {
			dfs(i, i);
		}
	}
	int cnt = 0;
	for (int i : joint) {
		cnt += i;
	}
	if (cnt == 0) {
		cout << 2 << '\n';
	}
	else {
		cout << 1 << '\n';
	}

	return 0;
}
```