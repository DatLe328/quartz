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
		int64_t n, k;	cin >> n >> k;
		
		int l = 1, r = 1e9;
		int cnt = 1;
		while (l <= r) {
			int mid = (l + r) / 2;
			if (k * mid >= n) {
				cnt = mid;
				r = mid - 1;
			}
			else {
				l = mid + 1;
			}
		}
		k *= cnt;
		int64_t res = (k + n - 1) / n;
		cout << res << '\n';
	}

	return 0;
}
```