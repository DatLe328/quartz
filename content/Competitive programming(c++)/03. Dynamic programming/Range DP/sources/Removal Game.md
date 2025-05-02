# Solution 1
```cpp
#include<bits/stdc++.h>
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

    int n;  cin >> n;
    vector<ll> a(n);
    for (auto &i : a) {
        cin >> i;
    }
    vector<vector<ll>> dp(n, vector<ll>(n, INT_MIN));
    for (int l = 1; l <= n; l++) {
        for (int i = 0; i < n - l + 1; i++) {
            int j = i + l - 1;
            if(i == j) {
                dp[i][j] = a[i];
            }
            else if (i + 1 == j) {
                dp[i][j] = max(a[i], a[j]);
            }
            else {
                auto pick_i = a[i] + min(dp[i + 2][j], dp[i + 1][j - 1]);
                auto pick_j = a[j] + min(dp[i][j - 2], dp[i + 1][j - 1]);
                dp[i][j] = max(pick_i, pick_j);
            }
        }
    }
    cout << dp[0][n - 1];

    return 0;
}
```
# Solution 2
```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 5e3 + 1;

int a[MAX_N];
ll dp[MAX_N][MAX_N];

ll memo(int i, int j) {
    if (i > j) return 0;
    if (dp[i][j] != -1) return dp[i][j];
    ll take_first = a[i] + min(memo(i + 2, j), memo(i + 1, j - 1));
    ll take_last = a[j] + min(memo(i, j - 2), memo(i + 1, j - 1));
    return dp[i][j] = max(take_first, take_last); 
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n;  cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    memset(dp, -1, sizeof dp);
    cout << memo(1, n);

    return 0;
}
```