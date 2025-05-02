```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 1e5 + 1;
const int BASE = 10;
const int D = 500;
ll MOD;

struct query {
	int id, l, r;
	query() {}
	query(int i, int l, int r) : id(i), l(l), r(r) {}
	int operator< (const query& b) {
		const query& a = *this;
		return make_pair(a.l / D, a.r) < make_pair(b.l / D, b.r);
	}
};
ll pow_mod(ll a, ll b) {
	a %= MOD;
	ll ret = 1;
	while (b) {
		if (b & 1) {
			ret = (ret * a) % MOD;
		}
		a = (a * a) % MOD;
		b /= 2;
	}
	return ret;
}

query qs[MAX_N];
ll ans[MAX_N], pwr[MAX_N], sum[MAX_N], rem[MAX_N], inv[MAX_N];
int q, n;
string s;

void solve1() {
	vector<vector<ll>> cnt(n + 1, vector<ll>(10, 0));
	vector<vector<ll>> sum(n + 1, vector<ll>(10, 0));
	for (int i = 1; i <= n; i++) {
		for (int d = 0; d <= 9; d++) {
			cnt[i][d] += cnt[i - 1][d];
			sum[i][d] += sum[i - 1][d];
		}
		cnt[i][s[i - 1] - '0']++;
		sum[i][s[i - 1] - '0'] += i;
	}
	for (int i = 1; i <= q; i++) {
		ll l, r;	cin >> l >> r;
		ll res = 0;
		for (int d = 0; d <= 9; d++) {
			if (d % MOD == 0) {
				res += sum[r][d] - sum[l - 1][d];
				res -= (l - 1) * (cnt[r][d] - cnt[l - 1][d]);
			}
		}
		cout << res << '\n';
	}
}
void solve2() {
	pwr[0] = 1;
	// String hashing
	for (int i = 1; i <= n; i++) {
		pwr[i] = (pwr[i - 1] * BASE % MOD);
		sum[i] = (sum[i - 1] * BASE + int(s[i - 1] - '0')) % MOD;
	}
	// Calculate Mod_inverse
	for (int i = 0; i <= n; i++) {
		inv[i] = pow_mod(pwr[i], MOD - 2);
	}
	// Compress value
	vector<ll> compress;
	for (int i = 0; i <= n; i++) {
		ll val = (sum[i] * inv[i]) % MOD;
		compress.push_back(val);
	}
	sort(compress.begin(), compress.end());
	compress.resize(unique(compress.begin(), compress.end()) - compress.begin());

	for (int i = 0; i <= n; i++) {
		ll val = (sum[i] * inv[i]) % MOD;
		rem[i] = lower_bound(compress.begin(), compress.end(), val) - compress.begin();
	}

	for (int i = 1; i <= q; i++) {
		int l, r;	cin >> l >> r;
		l--;
		qs[i] = query(i, l, r);
	}
	sort(qs + 1, qs + 1 + q);
	vector<int> cnt(compress.size() + 1, 0);
	int curL = 1, curR = 0;
	ll cur_ans = 0;
	for (int i = 1; i <= q; i++) {
		auto [id, l, r] = qs[i];
		while (curL < l) {
			cnt[rem[curL]]--;
			cur_ans -= cnt[rem[curL]];
			curL++;
		}
		while (curL > l) {
			curL--;
			cur_ans += cnt[rem[curL]];
			cnt[rem[curL]]++;
		}
		while (curR < r) {
			curR++;
			cur_ans += cnt[rem[curR]];
			cnt[rem[curR]]++;
		}
		while (curR > r) {
			cnt[rem[curR]]--;
			cur_ans -= cnt[rem[curR]];
			curR--;
		}
		ans[id] = cur_ans;
	}
	for (int i = 1; i <= q; i++) {
		cout << ans[i] << '\n';
	}
		
}
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	cin >> MOD >> s >> q;
	n = s.size();
	if (MOD == 2 || MOD == 5) {
		solve1();
	}
	else {
		solve2();
	}

	return 0;
}

```