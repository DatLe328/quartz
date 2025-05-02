# Solution 1: Bottom up
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

    int tt; cin >> tt;
    while (tt--) {
        int n, k;   cin >> n >> k;
        vector<vector<int64_t>> dp(1 << n, vector<int64_t>(n + 1));
        dp[0][0] = 1;
        for (int mask = 0; mask < (1 << n); mask++) {
            for (int i = 1; i <= n; i++) {
                if (mask & (1 << (i - 1))) continue;

                for (int j = 0; j <= n; j++) {
                    if (j != 0 && abs(i - j) > k) continue;
                    int new_mask = mask | (1 << (i - 1));
                    dp[new_mask][i] += dp[mask][j];
                }
            }
        }
        int64_t ans = 0;
        for (int i = 1; i <= n; i++) {
            ans += dp[(1 << n) - 1][i];
        }
        cout << ans << '\n';
    }

    return 0;
}
```
# Solution 2: Top down
```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 15;
ll dp[1 << MAX_N][MAX_N];
int n, k;

ll memo(int mask, int prev) {
    if (__builtin_popcount(mask) == n) return 1;
    if (dp[mask][prev] != -1) return dp[mask][prev];

    ll ret = 0;
    for (int i = 1; i <= n; i++) {
        if (prev != 0 && abs(i - prev) > k) continue;
        if ((mask & (1 << (i - 1))) == 0) {
            ret += memo(mask | (1 << (i - 1)), i);
        }
    }
    return dp[mask][prev] = ret;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int tt; cin >> tt;
    while (tt--) {
        cin >> n >> k;
        memset(dp, -1, sizeof dp);
        cout << memo(0, 0) << '\n';
    }

    return 0;
}
```