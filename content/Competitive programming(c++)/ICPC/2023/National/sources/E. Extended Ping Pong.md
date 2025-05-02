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

    int tt; cin >> tt;
    while (tt--) {
        int n, a, b;    cin >> n >> a >> b;
        int ans = 0;
        function<int(int, int)> solve = [&] (int u, int v) -> int {
            if (u > a || v > b) return 0;
            if (abs(u - v) >= 2 && max(u, v) >= n) {
                return u == a && v == b;
            }
            return solve(u + 1, v) + solve(u, v + 1);
        };
        cout << solve(0, 0) << '\n';
    }

    return 0;
}

```