```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

string l, r;
ll dp[20][2][2][2];  // [digit][tight_l][tight_r][leading_zero]

ll memo(int pos = 0, bool lower = 1, bool upper = 1, bool leading = 1) {
    if (pos == (int)l.size()) return 1;

    ll &ret = dp[pos][lower][upper][leading];
    if (~ret) return ret;

    int start = lower ? l[pos] - '0' : 0;
    int end = upper ? r[pos] - '0' : 9;

    ret = 0;
    for (int digit = start; digit <= end; digit++) {
        bool next_lower = lower && (digit == start);
        bool next_upper = upper && (digit == end);
        
        if (!digit && leading) {
            ret = max(ret, memo(pos + 1, next_lower, next_upper, 1));
        } else {
            ret = max(ret, memo(pos + 1, next_lower, next_upper, 0) * digit);
        }
    }
    return ret;
}

void print(int pos = 0, bool lower = 1, bool upper = 1, bool leading = 1) {
    if (pos == (int)l.size()) return;
    
    ll ret = dp[pos][lower][upper][leading];
    int start = lower ? l[pos] - '0' : 0;
    int end = upper ? r[pos] - '0' : 9;

    for (int digit = start; digit <= end; digit++) {
        bool next_lower = lower && (digit == start);
        bool next_upper = upper && (digit == end);

        if ((!digit && leading && ret == memo(pos + 1, next_lower, next_upper, 1)) ||
            (ret == memo(pos + 1, next_lower, next_upper, 0) * digit)) {
            if (digit || !leading) cout << digit;
            print(pos + 1, next_lower, next_upper, leading && digit == 0);
            return;
        }
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> l >> r;
    l = string(r.size() - l.size(), '0') + l;
    memset(dp, -1, sizeof dp);
    memo();
    print();
    return 0;
}

```