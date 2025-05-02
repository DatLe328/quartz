```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const ll MOD = 1e9 + 7;
ll dp[1001][1 << 10];
int n, m;

void fill_column(int column, int idx, int mask, int next_mask) {
    if (idx >= n) {
        dp[column + 1][next_mask] = (dp[column + 1][next_mask] + dp[column][mask]) % MOD;
        return;
    }
    
    int my_mask = 1 << idx;
    if (mask & my_mask) {
        // Current cell is occupied, move to the next cell
        fill_column(column, idx + 1, mask, next_mask);
    } else {
        // Place a tile vertically
        fill_column(column, idx + 1, mask, next_mask | my_mask);
        
        // Place a tile horizontally if possible
        if (idx + 1 < n && !(mask & (my_mask << 1))) {
            fill_column(column, idx + 2, mask, next_mask);
        }
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> m;
    dp[0][0] = 1;
    for (int column = 0; column < m; column++) {
        for (int mask = 0; mask < (1 << n); mask++) {
            if (dp[column][mask] > 0) {
                fill_column(column, 0, mask, 0);
            }
        }
    }
    cout << dp[m][0] << '\n';

    return 0;
}

```