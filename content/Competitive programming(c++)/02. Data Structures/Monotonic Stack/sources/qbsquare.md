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

    int n, m;   cin >> n >> m;
    vector<vector<int>> a(n + 1, vector<int>(m + 1));
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            cin >> a[i][j];
        }
    }
    vector<vector<int>> dp(n + 1, vector<int>(m + 1, 1));
    int ans = 0;
    for (int i = 2; i <= n; i++) {
        for (int j = 2; j <= m; j++) {
            if (a[i][j] == a[i - 1][j] && a[i][j] == a[i - 1][j - 1] && a[i][j] == a[i][j - 1]) {
                dp[i][j] = min({dp[i - 1][j], dp[i - 1][j - 1], dp[i][j - 1]}) + 1;
            }
            ans = max(ans, dp[i][j]);
        }
    }
    // for (int i = 1; i <= n; i++) {
    //     for (int j = 1; j <= m; j++) {
    //         cout << dp[i][j] << ' ';
    //     }
    //     cout << '\n';
    // }
    cout << ans;

    return 0;
}

```