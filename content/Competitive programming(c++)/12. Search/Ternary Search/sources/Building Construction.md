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

	int tt;	cin >> tt;
	while (tt--) {
		int n;	cin >> n;
		vector<pair<int, int>> a(n);
		for (int i = 0; i < n; i++) {
			cin >> a[i].first;
		}
		for (int i = 0; i < n; i++) {
			cin >> a[i].second;
		}
		int N_ITER = 100;
		double left = 0.0;
		double right = 10000.0;
		auto f = [&] (double x) -> int64_t {
			int64_t cost = 0;
			for (int i = 0; i < n; i++) {
				cost += fabs(x - a[i].first) * a[i].second;
			}
			// cout << cost << '\n';
			return cost;
		};
		for (int i = 0; i < N_ITER; i++) {

			double x1 = left + (right - left) / 3.0;
			double x2 = right - (right - left) / 3.0;

			if (f(x1) > f(x2)) left = x1;
			else right = x2;
		}
		cout << min(f(floor(left)), f(ceil(right))) << '\n';
	}

	return 0;
}
```