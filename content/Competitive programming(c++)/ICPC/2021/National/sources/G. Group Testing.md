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
	vector<int> a(n);
	for (auto &i : a) {
		cin >> i;
	}
	sort(a.begin(), a.end());
	int mx = 0, mn = 0;
	for (int i = 0; i < k; i++) {
		if (a[i] > 1) {
			mn += a[i];
		}
	}
	for (int i = n - k; i < n; i++) {
		if (a[i] > 1) {
			mx += a[i];
		}
	}
	cout << mn << ' ' << mx;

	return 0;
}

```