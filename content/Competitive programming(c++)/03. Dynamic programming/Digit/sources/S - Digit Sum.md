```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const ll MOD = 1e9 + 7;
ll dp[10001][101][2];
int d;

ll memo(string num, int pos, int sum, bool tight) {
    if (pos == int(num.size()) ) {
        return (sum % d == 0) ? 1 : 0;
    }
    if (dp[pos][sum][tight] != -1) {
        return dp[pos][sum][tight];
    }
    int lim = (tight ? int(num[pos] - '0') : 9);
    ll ret = 0;
    for (int i = 0; i <= lim; i++) {
        bool next_tight = (i == lim) && tight;
        ret = (ret + memo(num, pos + 1, (sum + i) % d, next_tight)) % MOD;
    }
    return dp[pos][sum][tight] = ret;
}

ll calc(string num) {
    memset(dp, -1, sizeof dp);
    return memo(num, 0, 0, true);
}

void solve() {
    string n;   cin >> n;
    cin >> d;
    // add MOD because we don't consider 0 as multiple of D
    cout << (calc(n) - 1 + MOD) % MOD;
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    solve();

    return 0;
}
```