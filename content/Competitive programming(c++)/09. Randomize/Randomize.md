```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

ll start = chrono::steady_clock::now().time_since_epoch().count();
mt19937_64 rng(start);

ll rand(ll l, ll r) {
	return uniform_int_distribution<int>(l, r)(rng);
}
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	vector<int> a{1,2,3,4,5,6,7,8,9};
	while (true) {
		ll current = chrono::steady_clock::now().time_since_epoch().count();
		if (current - start >= 0.97 * 1e9)
			break;
		shuffle(a.begin(), a.end(), rng);
		cout << rand(1, 10) << '\n';
	}

	return 0;
}
```