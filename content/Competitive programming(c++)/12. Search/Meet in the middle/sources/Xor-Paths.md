```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

int n, m;
ll ans = 0;
ll k, a[21][21];
map<ll, int> mp[21][21];
void try_x(int i, int j, ll val, int cnt) {
	val ^= a[i][j];
	if (cnt == 0) {
		mp[i][j][val]++;
		return;
	}
	if (i < n) {
		try_x(i + 1, j, val, cnt - 1);
	}
	if (j < m) {
		try_x(i, j + 1, val, cnt - 1);
	}
}
void try_y(int i, int j, ll val, int cnt) {
	if (cnt == 0) {
		if (mp[i][j].find(k ^ val) != mp[i][j].end()) {
			ans += mp[i][j][val ^ k];
		}
		return;
	}
	val ^= a[i][j];
	if (i > 1) {
		try_y(i - 1, j, val, cnt - 1);
	}
	if (j > 1) {
		try_y(i, j - 1, val, cnt - 1);
	}
}

int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	cin >> n >> m >> k;
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= m; j++) {
			cin >> a[i][j];
		}
	}
	int cnt = (n + m - 2);
	try_x(1, 1, 0, cnt / 2);
	try_y(n, m, 0, cnt - cnt / 2);
	cout << ans;

	return 0;
}
```