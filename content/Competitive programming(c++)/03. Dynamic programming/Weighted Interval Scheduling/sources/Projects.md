```cpp
#include <bits/stdc++.h>
#define ll long long
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
    cin >> n;
    using ar = array<ll, 3>;
    vector<ar> a;
    for (int i = 0; i < n; i++) {
        int x, y, z;    cin >> x >> y >> z;
        a.push_back({y, x, z});
    }
    sort(a.begin(), a.end());
    vector<ll> dp(n + 1);
    for (int i = 1; i <= n; i++) {
        ll start = a[i - 1][1];
        ll reward = a[i - 1][2];
        ar p = {start, 0, 0};
        int nxt = lower_bound(a.begin(), a.end(), p) - a.begin();
        dp[i] = max(dp[i - 1], reward + dp[nxt]);
    }
    cout << dp[n];

    return 0;
}

```