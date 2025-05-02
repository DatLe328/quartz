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
    int total = 0;
    for (auto &i : a) {
        cin >> i;
        total += i;
    }
    vector<bool> dp(total + 1, false);
    dp[0] = true;
    int cnt = 0;
    for (int i = 0; i < n; i++) {
        for (int j = total; j >= a[i]; j--) {
            if (dp[j - a[i]]) {
                if (!dp[j]) cnt++;
                dp[j] = true;
            }
        }
    }
    cout << cnt << '\n';
    for (int i = 1; i <= total; i++) {
        if (dp[i]) {
            cout << i << ' ';
        }
    }
 
    return 0;
}
```