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
	vector<int> res;
	for (int i = 9; i > 1; i--) {
		while (n % i == 0) {
			res.push_back(i);
			n /= i;
		}
	}
	if (n != 1 || res.size() > k) {
		cout << -1;
		return 0;
	}
	while (res.size() < k) {
		res.push_back(1);
	}
	reverse(res.begin(), res.end());
	for (int i : res) {
		cout << i;
	}

	return 0;
}

```