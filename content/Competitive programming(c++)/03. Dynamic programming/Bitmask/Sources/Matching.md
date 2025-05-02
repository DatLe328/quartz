```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 21;
const ll MOD = 1e9 + 7;

ll dp[1 << MAX_N];
int a[MAX_N][MAX_N];
int n;

// Top down approach
int calc(int mask) {
    int k = __builtin_popcount(mask);
    if (k == n) {
        return 1;
    }
    if (dp[mask] != -1) {
        return dp[mask];
    }
    ll ans = 0;
    for (int i = 0; i < n; i++) {
        if (a[i][k] && !(mask & (1 << i))) {
            ans += calc(mask | (1 << i));
            ans %= MOD;
        }
    }
    return dp[mask] = ans;
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> a[i][j];
        }
    }
	
	// Bottom up approach
    dp[0] = 1;
    for (int mask = 0; mask < (1 << n); mask++) {
        int k = __builtin_popcount(mask);
        for (int i = 0; i < n; i++) {
            if (a[i][k] && !(mask & (1 << i))) {
                dp[mask | (1 << i)] += dp[mask];
                dp[mask | (1 << i)] %= MOD;
            }
        }
    }
    cout << dp[(1 << n) - 1];

    return 0;
}

```