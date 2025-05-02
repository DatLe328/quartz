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
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int tt; cin >> tt;
    while (tt--) {
        int n;  cin >> n;
        vector<ll> dp(71, 0);
        dp[0] = 1;
        for (int i = 0; i < n; i++) {
            int x;  cin >> x;
            vector<ll> tmp_dp = dp;
            for (int j = 0; j <= 70; j++) {
                ll res = __gcd(j, x);
                tmp_dp[res] = (tmp_dp[res] + dp[j]) % MOD;
            }
            swap(tmp_dp, dp);
        }
        ll ans = 0;
        for (int i = 1; i <= 70; i++) {
            ll w = (dp[i] * i) % MOD;
            ans = (ans + w) % MOD;
        }
        cout << ans << '\n';
    }

    return 0;
}
```