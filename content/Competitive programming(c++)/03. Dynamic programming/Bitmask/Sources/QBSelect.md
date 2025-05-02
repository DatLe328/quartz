```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 1e4 + 2;
ll dp[1 << 4][MAX_N];
ll a[4][MAX_N];
int n;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n;
    // Check if the table contains only negative values
    // We must select at least 1 node
    bool neg = true;
    ll mx = INT_MIN;
    for (int i = 0; i < 4; i++) {
        for (int j = 1; j <= n; j++) {
            cin >> a[i][j];
            if (a[i][j] > 0) {
                neg = false;
            }
            if (a[i][j] < 0) {
                mx = max(mx, a[i][j]);
            }
        }
    }
    if (neg) {
        cout << mx << '\n';
        return 0;
    }
    // Get the appropriate bitmask
    vector<int> res;
    for (int mask = 0; mask < (1 << 4); mask++) {
        if (__builtin_popcount(mask) > 3) continue;
        bool safe = true;
        for (int k = 0; k < 4; k++) {
            if (mask & (1 << k) && mask & (1 << (k + 1))) {
                safe = false;
                break;
            }
        }
        if (!safe) continue;
        res.push_back(mask);
    }
    // Check masks for two columns
    auto check = [] (int bt1, int bt2) -> bool {
        bool safe = true;
        for (int i = 0; i < 4; i++) {
            if ((bt1 & (1 << i)) && (bt2 & (1 << i))) {
                safe = false;
                break;
            }
        }
        return safe;
    };
    for (int col = 1; col <= n + 1; col++) {
        for (int mask : res) {
            int val = 0;
            for (int i = 0; i < 4; i++) {
                if (mask & (1 << i)) {
                    val += a[i][col];
                }
            }
            for (int prev : res) {
                if (check(mask, prev)) {
                    dp[mask][col] = max(dp[mask][col], dp[prev][col - 1] + val);
                }
            }
        }
    }
    
    ll ans = 1;
    for (int i = 0; i < (1 << 4); i++) {
        ans = max(ans, dp[i][n + 1]);
    }
    cout << ans;
    return 0;
}
```