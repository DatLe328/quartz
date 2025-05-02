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

    int n;  cin >> n;
    vector<vector<int>> dist(n, vector<int>(n));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> dist[i][j];
        }
    }
    int ans = INT_MAX;
    vector<vector<int>> dp(1 << n, vector<int>(n, 1e9));
    for (int i = 0; i < n; i++) {
        dp[1 << i][i] = 0;
    }
    for (int mask = 1; mask < (1 << n); mask++) {
        for (int u = 0; u < n; u++) {
            if ((mask & (1 << u)) == 0) continue;
            int prev = mask ^ (1 << u);
            for (int v = 0; v < n; v++) {
                if (mask & (1 << v)) {
                    dp[mask][u] = min(dp[mask][u], dp[prev][v] + dist[v][u]);
                }
            }
        }
    }
    for (int i = 0; i < n; i++) {
        ans = min(ans, dp[(1 << n) - 1][i]);
    }
    cout << ans;


    return 0;
}
```