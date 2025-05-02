```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

ll dp[19][10][2][2];
vector<int> num;
bool first;

ll memo(int pos, int last_num, bool tight, bool all_z) {
    if (pos == (int)num.size()) {
        return 1;
    }
    if (dp[pos][last_num][tight][all_z] != -1) {
        return dp[pos][last_num][tight][all_z];
    }
    int lim = (tight ? num[pos] : 9);
    ll ret = 0;
    for (int i = 0; i <= lim; i++) {
        if (!all_z && i == last_num) continue;
        bool next_tight = (i == lim && tight);
        bool next_z = (all_z && (i == 0));
        ret += memo(pos + 1, i, next_tight, next_z);
    }
    return dp[pos][last_num][tight][all_z] = ret;
}
ll calc(ll i) {
    num.clear();
    while (i) {
        num.push_back(i % 10);
        i /= 10;
    }
    memset(dp, -1, sizeof dp);
    reverse(num.begin(), num.end());
    return memo(0, 0, true, true);
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    ll l, r; cin >> l >> r;
    cout << calc(r) - calc(l - 1);

    return 0;
}
```