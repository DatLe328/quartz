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
	vector<vector<char>> a(n, vector<char>(m));
	pair<int, int> s, e;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < m; j++) {
			cin >> a[i][j];
			if (a[i][j] == 'A') {
				s = {i, j};
			}
			if (a[i][j] == 'B') {
				e = {i, j};
			}
		}
	}
	int dx[]{0, -1, 1, 0};
	int dy[]{-1, 0, 0, 1};
	queue<pair<int, int>> q;
	vector<vector<pair<int, int>>> par(n, vector<pair<int,int>>(m));
	vector<vector<int>> dist(n, vector<int>(m, -1));
	q.push(s);
	par[s.first][s.second] = {s.first, s.second};
	dist[s.first][s.second] = 0;
	while (q.size()) {
		auto [i, j] = q.front();	q.pop();
		for (int k = 0; k < 4; k++) {
			int y = i + dy[k];
			int x = j + dx[k];
			if (y < 0 || x < 0 || y == n || x == m || a[y][x] == '#') continue;
			if (dist[y][x] == -1) {
				dist[y][x] = dist[i][j] + 1;
				par[y][x] = {i, j};
				q.push({y, x});
			}
		}
	}
	if (dist[e.first][e.second] == -1) {
		cout << "NO\n";
	}
	else {
		auto [i, j] = e;
		cout << "YES\n";
		cout << dist[i][j] << '\n';
		string trace = "";
		while (par[i][j] != make_pair(i, j)) {
			auto [y, x] = par[i][j];
			if (y == i - 1) {
				trace.push_back('D');
			}
			if (y == i + 1) {
				trace.push_back('U');
			}
			if (x == j + 1) {
				trace.push_back('L');
			}
			if (x == j - 1) {
				trace.push_back('R');
			}
			i = y, j = x;
		}
		reverse(trace.begin(), trace.end());
		cout << trace;
	}
 
	return 0;
}

```