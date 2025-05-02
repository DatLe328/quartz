```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const ll MOD = 998244353;
ll dp[17];
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    dp[0] = dp[1] = 1 ;
    for (int i = 2; i <= 16; i++) {
        for (int j = 0; j < i; j++) {
            dp[i] = (dp[i] + (1 << j) * dp[j] * dp[i - j - 1]) % MOD;
        }
    }
    int n;  cin >> n;
    cout << dp[n - 1];

    return 0;
}

```