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
	vector<double> a(n);
	for (auto &i : a) {
		cin >> i;
	}
	int N_ITER = 120;
	double left = -1e9;
	double right = 1e9;
	cout << fixed << setprecision(12);
	auto f = [&] (double x) -> double {
		double res = 0;
		double cur = 0;
		for (int i = 0; i < n; i++) {
			cur += a[i] - x;
			res = max(res , cur);
			if (cur < 0) cur = 0;
		}
		cur = 0;
		for (int i = 0; i < n; i++) {
			cur += x - a[i];
			res = max(res , cur);
			if (cur < 0) cur = 0;
		}
		return res;
	};
	for (int i = 0; i < N_ITER; i++) {

		double x1 = left + (right - left) / 3.0;
		double x2 = right - (right - left) / 3.0;

		if (f(x1) < f(x2)) right = x2;
		else left = x1;
	}

	cout << f(left);

	return 0;
}
```