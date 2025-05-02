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

    int n, m;   cin >> n >> m;
    vector<int> a(n);
    for (auto &i : a) {
        cin >> i;
    }
    int ans = 0;
    bool safe = true;
    for (int t = 0; t < m; t++) {
        int x, y, z;    cin >> x >> y >> z;
        if (!safe) continue;
        int cur_l = 0, cur_r = -1;
        for (int i = 0; i < n; i++) {
            while (cur_l <= i && a[i] - a[cur_l] > y) {
                cur_l++;
            }
            while (cur_r < i && a[i] - a[cur_r + 1] >= x) {
                cur_r++;
            }
            z -= (cur_r - cur_l + 1);
            if (z <= 0) {
                ans = max(ans, a[i]);
                break;
            }
        }
        if (z > 0) safe = false;
    }
    if (safe) {
        cout << ans << '\n';
    }
    else {
        cout << -1 << '\n';
    }

    return 0;
}
```