```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;

ll dp[19][4][2];
vector<int> num;

ll memo(int idx, int non_zero, bool tight) {
    if (non_zero > 3) return 0;
    if (idx == num.size()) return 1;

    if (dp[idx][non_zero][tight] != -1) return dp[idx][non_zero][tight];

    ll ret = 0;
    int lim = tight ? num[idx] : 9;

    for (int i = 0; i <= lim; i++) {
        bool next_tight = tight && (i == lim);
        int next_non_zero = non_zero + (i != 0);
        ret += memo(idx + 1, next_non_zero, next_tight);
    }

    return dp[idx][non_zero][tight] = ret;
}

ll calc(ll n) {
    if (n == 0) return 1;
    num.clear();
    while (n) {
        num.push_back(n % 10);
        n /= 10;
    }
    reverse(num.begin(), num.end());
    memset(dp, -1, sizeof dp);
    return memo(0, 0, true);
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int tt;
    cin >> tt;
    while (tt--) {
        ll l, r;
        cin >> l >> r;
        cout << calc(r) - calc(l - 1) << '\n';
    }

    return 0;
}
```