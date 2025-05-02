```cpp
#include<bits/stdc++.h>
#define ll long long
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
	map<int, int> cnt;
	vector<int> min_prime(1e7 + 1);
	iota(min_prime.begin(), min_prime.end(), 0);
	for (int i = 2; i <= sqrt(1e7); i++) {
		if (min_prime[i] == i) {
			for (int j = i * i; j <= 1e7; j += i) {
				if (min_prime[j] == j) {
					min_prime[j] = i;
				}
			}
		}
	
	}

	ll ans = 0;
	for (int i = 0; i < n; i++) {
		int x;	cin >> x;
		ll res = 1;
		while (x > 1) {
			int cur = min_prime[x];
			res *= cur;
			while (cur == min_prime[x]) {
				x /= cur;
			}
		}
		ans += cnt[res];
		cnt[res]++; 
	}
	cout << ans;

	return 0;
}
```