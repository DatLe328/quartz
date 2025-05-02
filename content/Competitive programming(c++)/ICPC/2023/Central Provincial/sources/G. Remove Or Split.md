```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 2e5 + 1;
int dp[MAX_N];

int mex(const vector<int>& a) {
    int n = (int)a.size();
    vector<int> c(n + 2, 0);
    for (int x : a) {
        if (x <= n + 1) c[x]++;
    }
    int ans = 0;
    while (c[ans]) ans++;
    return ans;
}

void init_grundy() {
    int n = 10000;
    dp[0] = 0;
    dp[1] = 1;
    for (int i = 2; i <= n; i++) {
        vector<int> x;
        for (int j = 1; j * 2 <= i; j++) {
            x.push_back(dp[j] ^ dp[i - j]);
        }
        for (int j = 0; j < i; ++j) x.push_back(dp[j]);
        dp[i] = mex(x);
    }
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    init_grundy();
    int n;  cin >> n;
    int nim_sum = 0;
    for (int i = 0; i < n; i++) {
        int x;  cin >> x;
        nim_sum ^= dp[x];
    }
    if (nim_sum) {
        cout << "Alice\n";
    }
    else {
        cout << "Bob\n";
    }

    return 0;
}
```