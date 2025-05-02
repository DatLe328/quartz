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
 
    int n;  cin >> n;
    int total = n * (n + 1) / 2;
    if (total & 1) {
        cout << 0;
        return 0;
    }
    debug(total);
    vector<vector<ll>> dp(n + 1, vector<ll>(total / 2 + 1));
    dp[0][0] = 1;
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j <= total / 2; j++) {
            dp[i][j] = dp[i - 1][j];
            if (i > j)  continue;
            dp[i][j] += dp[i - 1][j - i];
            dp[i][j] %= MOD;
        }
    }
    debug(dp);
    cout << dp[n - 1][total / 2];
 
    return 0;
}
```