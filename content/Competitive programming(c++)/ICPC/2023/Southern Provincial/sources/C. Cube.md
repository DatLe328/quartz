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

	int n, k;	cin >> n >> k;
	set<tuple<int, int, int>> x0, y0, z0;
	for (int i = 0; i < k; i++) {
		int x, y, z;	cin >> x >> y >> z;
		if (x == 0) x0.insert({x, y, z});
		else if (y == 0) y0.insert({x, y, z});
		else z0.insert({x, y, z});
	}
	long long ans = 0;
	map<int, int> cnt_x, cnt_y, cnt_z;
	map<pair<int, int>, int> cnt_xy;
	for (auto [x, y, z] : x0) {
		ans += n;
		cnt_z[z]++;
		cnt_y[y]++;
	}
	for (auto [x, y, z] : y0) {
		ans += n - cnt_z[z];
		cnt_x[x]++;
		for (auto [x2, y2, z2] : x0) {
			if (z == z2) {
				cnt_xy[make_pair(x, y2)]++;
			}
		}
	}
	for (auto [x, y, z] : z0) {
		ans += n - cnt_x[x] - cnt_y[y] + cnt_xy[make_pair(x, y)];
	}
	cout << ans;

	return 0;
}
```