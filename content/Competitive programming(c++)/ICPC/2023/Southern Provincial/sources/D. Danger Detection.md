```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int INF = 1e9;
const int dx[4] = {-1, 0, 1, 0};
const int dy[4] = {0, 1, 0, -1};

int get_n_dangerous(const vector<vector<int>>& a, int G) {
    int n = a.size();
    vector<vector<int>> cost(n, vector<int>(n));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (a[i][j] < G) {
                cost[i][j] = 1;
            } else {
                cost[i][j] = 0;
            }
        }
    }
    vector<vector<int>> d(n, vector<int>(n, INF));
    d[0][0] = 0;
    typedef pair<int, pair<int, int>> i2;
    priority_queue<i2, vector<i2>, greater<i2>> Q;
    Q.push({0, {0, 0}});
    while ((int)Q.size() != 0) {
        i2 it = Q.top();
        Q.pop();
        int du = it.first, u = it.second.first, v = it.second.second;
        if (du != d[u][v]) {
            continue;
        }
        for (int h = 0; h < 4; h++) {
            int x = u + dx[h];
            int y = v + dy[h];
            if (x >= 0 && x < n && y >= 0 && y < n && d[x][y] > d[u][v] + cost[x][y]) {
                d[x][y] = d[u][v] + cost[x][y];
                Q.push({d[x][y], {x, y}});
            }
        }
    }
    return d[n - 1][n - 1];
}
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	int t;	cin >> t;
	if (t == 1) {
		int n, g;	cin >> n >> g;
		vector<vector<int>> a(n, vector<int>(n));
		for (auto &i : a) {
			for (auto &j : i) {
				cin >> j;
			}
		}
		cout << get_n_dangerous(a, g);
	}
	else {
		int n;	cin >> n;
		vector<vector<int>> a(n, vector<int>(n));
		for (auto &i : a) {
			for (auto &j : i) {
				cin >> j;
			}
		}
		int l = 1, r = 5000;
		int ans = -1;
		while (l <= r) {
			int mid = (l + r) / 2;
			if (get_n_dangerous(a, mid) == 0) {
				ans = mid;
				l = mid + 1;
			}
			else {
				r = mid - 1;
			}
		}
		cout << ans;
	}

	return 0;
}
```