```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

ll dp[50];
int n;
vector<int> res;

ll memo(int pos, int prev) {
	if (pos == n) {
		return 1;
	}
	if (dp[pos]) return dp[pos];
	ll ret = 0;
	ret += memo(pos + 1, prev);
	if (pos + 1 != prev) {
		ret += memo(pos + 1, pos + 1);
	}
	return dp[pos] = ret;
}
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	cin >> n;
	cout << memo(0, 1);

	return 0;
}

```