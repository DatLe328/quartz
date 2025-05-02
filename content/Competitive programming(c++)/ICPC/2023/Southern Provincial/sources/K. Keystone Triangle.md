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

	int n;	cin >> n;
	vector<int> a(n);
	for (auto &i : a) {
		cin >> i;
	}
	vector<int> left(n), right(n);
	left[0] = min(1, a[0]);
	right[n - 1] = min(1, a[n - 1]);
	for (int i = 1; i < n; i++) {
		left[i] = min(left[i - 1] + 1, a[i]);
	}
	for (int i = n - 2; i >= 0; i--) {
		right[i] = min(right[i + 1] + 1, a[i]);
	}
	int ans = 0;
	for (int i = 0; i < n; i++) {
		ans = max(ans, min(left[i], right[i]));
	}
	cout << ans;
	return 0;
}
```