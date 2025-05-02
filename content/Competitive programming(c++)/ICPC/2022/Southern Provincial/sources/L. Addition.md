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

    int n;  
    while (true) {
        cin >> n;
        if (n == 0) break;
        string a, b, c;  cin >> a >> b >> c;
        reverse(a.begin(), a.end());
        reverse(b.begin(), b.end());
        reverse(c.begin(), c.end());
        vector<vector<int>> dp(n + 1, vector<int>(2, 1e9));
        dp[0][0] = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < 2; j++) {
                int d = int(a[i] - '0') + int(b[i] - '0') + j;
                if (d % 10 == int(c[i] - '0')) {
                    int rem = d / 10;
                    dp[i + 1][rem] = dp[i][j];
                }
            }
            dp[i + 1][0] = min(dp[i + 1][0], dp[i][0] + 1);
            dp[i + 1][1] = min(dp[i + 1][1], dp[i][1] + 1);
        }
        cout << dp[n][0] << '\n';
    }

    return 0;
}
```