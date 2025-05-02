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
    vector<int> a(n);
    for (auto &i : a) {
        cin >> i;
    }
    vector<int> dp(n);
    int best = 0, cur = 0;
    int fee = 0;
    for (int i = 0; i < n; i++) {
        bool only_cur = false;
        if (cur + a[i] > a[i]) {
            cur += a[i];
        }
        else {
            only_cur = true;
            cur = a[i];
        }
        if (cur > 0) {
            dp[i] = cur;
            if (only_cur) {
                fee = max(a[i], 0);
            }
            else {
                fee = max(fee, a[i]);
            }
        }
        best = max(best, dp[i] - fee);
    }
    cout << best;

    return 0;
}
```