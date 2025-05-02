```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int INF = 1e9;
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n, x;   cin >> n >> x;
    vector<int> a(n);
    for (auto &i : a) {
        cin >> i;
    }
    vector<int> dp(x + 1, INF);
    dp[0] = 0;
    for (int i = 1; i <= x; i++) {
        for (int j = 0; j < n; j++) {
            if (a[j] > i) continue;
            dp[i] = min(dp[i], dp[i - a[j]] + 1);
        }
    }
    debug(dp);
    cout << (dp[x] == INF ? -1 : dp[x]);
    return 0;
}
```