```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 5001;
int dp[MAX_N][MAX_N];
bool is_pal[MAX_N][MAX_N];

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    string s;   cin >>s;
    int n = (int)s.size();
    s = " " + s;
    for (int i = 1; i <= n; i++) {
        dp[i][i] = 1;
        is_pal[i][i] = true;
        is_pal[i + 1][i] = true;
    }
    for (int len = 2; len <= n; len++) {
        for (int i = 1; i <= n - len + 1; i++) {
            int j = i + len - 1;
            if (is_pal[i + 1][j - 1] && s[i] == s[j]) {
                is_pal[i][j] = true;
            }
            dp[i][j] = dp[i][j - 1] + dp[i + 1][j]  - dp[i + 1][j - 1] + is_pal[i][j];
        }
    }
    int q;  cin >> q;
    while (q--) {
        int l, r;   cin >> l >> r;
        cout << dp[l][r] << '\n';
    }

    return 0;
}
```