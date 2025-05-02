```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 1e5 + 1;
int a[MAX_N], b[MAX_N], n, m;

ll f(ll mid) {
	ll ret = 0;
	for (int i = 1; i <= n; i++) {
		if (a[i] < mid) {
			ret += mid - a[i];
		}
	}
	for (int i = 1; i <= m; i++) {
		if (b[i] > mid) {
			ret += b[i] - mid;
		}
	}
	// cout << mid << ' ' << ret << '\n';
	return ret;
}
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	cin >> n >> m;
	for (int i = 1; i <= n; i++) {
		cin >> a[i];
	}
	for (int i = 1; i <= m; i++) {
		cin >> b[i];
	}
	ll left = 1;
	ll right = 1e12;
	while (right - left > 4) {
		ll x1 = left + (right - left) / 3.0;
		ll x2 = right - (right - left) / 3.0;
		if (f(x1) > f(x2)) left = x1;
		else right = x2;
	}
	ll ans = 1e18;
	for (int i = left; i <= right; i++) {
		ans = min(ans, f(i));
	}
	cout << ans;

	return 0;
}
```