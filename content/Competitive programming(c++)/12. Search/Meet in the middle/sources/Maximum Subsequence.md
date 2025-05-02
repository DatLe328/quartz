```cpp
#include<bits/stdc++.h>
using namespace std;

const int MAX_N = 36;
int64_t a[MAX_N];
int n, m;
vector<int64_t> bleft, bright;

void do_left(int idx, int64_t sum) {
	if (idx == n / 2) {
		bleft.push_back(sum);
		return;
	}
	do_left(idx + 1, sum);
	do_left(idx + 1, (sum + a[idx]) % m);
}

void do_bright(int idx, int64_t sum) {
	if (idx == n) {
		bright.push_back(sum);
		return;
	}
	do_bright(idx + 1, sum);
	do_bright(idx + 1, (sum + a[idx]) % m);
}
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	cin >> n >> m;
	for (int i = 0; i < n; i++) {
		cin >> a[i];
	}
	do_left(0, 0);
	do_bright(n / 2, 0);
	sort(bright.begin(), bright.end(), greater<int64_t>());
	int64_t ans = 0;
	for (auto i : bleft) {
		auto it = upper_bound(bright.begin(), bright.end(), m - i, greater<int64_t>());
		if (it != bright.end()) {
			ans = max(ans, i + *it);
		}
	}
	cout << ans;

	return 0;
}
```