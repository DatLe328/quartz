```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 16;
int a[MAX_N][MAX_N];
ll dp[1 << MAX_N];
ll cost[1 << MAX_N];
int n;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> a[i][j];
        }
    }
    for (int mask = 1; mask < (1 << n); mask++) {
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (mask & (1 << i) && mask & (1 << j)) {
                    cost[mask] += a[i][j];
                }
            }
        }
    }
    for (int mask = 0; mask < (1 << n); mask++) {
        int reverse_mask = ((1 << n) - 1) ^ mask;
        int k = reverse_mask;
        // loop all reverse_mask's submask
        while (k) {
            dp[mask ^ k] = max(dp[mask ^ k], dp[mask] + cost[k]);
            k = (k - 1) & reverse_mask;
        }
    }

    cout << dp[(1 << n) - 1] << '\n';

    return 0;
}

```