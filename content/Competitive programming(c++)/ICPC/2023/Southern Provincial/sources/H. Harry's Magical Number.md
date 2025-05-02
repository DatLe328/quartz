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

	int a, b, c, d;	cin >> a >> b >> c >> d;
	vector<int> res;
	for (int i = 1; i <= sqrt(c); i++) {
		if (c % i == 0) {
			if (i % a == 0 && i % b != 0 && d % i != 0) {
				res.push_back(i);
			}
			if (i * i != c) {
				int tmp = c / i;
				if (tmp % a == 0 && tmp % b != 0 && d % tmp != 0) {
					res.push_back(tmp);
				}
			}
		}
	}
	if (res.size()) {
		cout << *min_element(res.begin(), res.end());
	}
	else {
		cout << -1;
	}

	return 0;
}
```