```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

ll dp[10][91][2];

ll memo(string num, int pos, int sum, bool tight) {
    if (pos == int(num.size()) ) {
        return sum;
    }
    if (dp[pos][sum][tight] != -1) {
        return dp[pos][sum][tight];
    }
    int lim = (tight ? int(num[pos] - '0') : 9);
    ll ret = 0;
    for (int i = 0; i <= lim; i++) {
        bool next_tight = (i == lim) && tight;
        ret += memo(num, pos + 1, sum + i, next_tight);
    }
    return dp[pos][sum][tight] = ret;
}

ll calc(string num) {
    memset(dp, -1, sizeof dp);
    return memo(num, 0, 0, true);
}

void solve() {
    int l, r;
    while (true) {
        cin >> l >> r;
        if (l == -1) break;
        cout << calc(to_string(r)) - calc(to_string(l - 1)) << '\n';
    }
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    solve();

    return 0;
}
```