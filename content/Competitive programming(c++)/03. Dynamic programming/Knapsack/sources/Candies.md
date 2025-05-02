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

    int n, k;   cin >> n >> k;
    vector<int> a(n);
    for (auto &i : a) {
        cin >> i;
    }
    vector<ll> dp(k + 1);
    dp[0] = 1;
    for (int i = 1; i <= n; i++) {
        vector<ll> pref(k + 1, 0);
        pref[0] = dp[0];
        for (int j = 1; j <= k; j++) {
            pref[j] = pref[j - 1] + dp[j];
        }
        vector<ll> new_dp(k + 1, 0);
        for (int j = 0; j <= k; j++) {
            int m = max(0, j - a[i - 1]);
            if (m > 0) {
                new_dp[j] = (pref[j] - pref[m - 1] + MOD) % MOD;
            }
            else {
                new_dp[j] = pref[j] % MOD;
            }
        }
        swap(dp, new_dp);
    }
    cout << dp[k];

    return 0;
}
```