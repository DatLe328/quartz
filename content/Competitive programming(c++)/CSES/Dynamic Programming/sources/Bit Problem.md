```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 1e5 + 1;
const int MAX_M = 20;

inline int inv(const int& x) {
    return x ^ ((1 << MAX_M) - 1);
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n;  cin >> n;
    vector<int> a(n);
    vector<int> dp(1 << MAX_M), dp_inv(1 << MAX_M);
    for (auto &i : a) {
        cin >> i;
        dp[i]++;
        dp_inv[inv(i)]++;
    }
    for (int i = 0; i < MAX_M; i++) {
        for (int mask = 0; mask < (1 << MAX_M); mask++) {
            if (mask & (1 << i)) {
                dp[mask] += dp[mask ^ (1 << i)];
                dp_inv[mask] += dp_inv[mask ^ (1 << i)];
            }
        }
    }
    for (int i : a) {
        cout << dp[i] << " " << dp_inv[inv(i)] << " " << n - dp[inv(i)] << "\n";
    }

    return 0;
}
```