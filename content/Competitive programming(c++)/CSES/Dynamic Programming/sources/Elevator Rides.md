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
 
	int n, x;	cin >> n >> x;
	vector<int> a(n);
	for (auto &i : a) {
		cin >> i;
	}
	vector<pair<int, int>> dp(1 << n, {0, 0});
	dp[0] = {1, 0};
	for (int mask = 1; mask < (1 << n); mask++) {
		pair<int, int> best = {INT_MAX, 0};
		for (int i = 0; i < n; i++) {
			if (mask & (1 << i)) {
				auto [cnt, w] = dp[mask ^ (1 << i)];
				if (w + a[i] > x) {
					best = min(best, {cnt + 1, a[i]});
				}
				else {
					best = min(best, {cnt, w + a[i]});
				}
			}
		}
		dp[mask] = best;
	}
	cout << dp[(1 << n) - 1].first;
 
	return 0;
}
```