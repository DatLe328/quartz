```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

mt19937 rnd(chrono::steady_clock().now().time_since_epoch().count());

int main() {
    auto dist = [&](int u, int v) {
        cout << "distance " << u << " " << v << endl;
        int x;
        cin >> x;
        return x;
    };

    auto subtree = [&](int u, int v) {
        cout << "subtree " << u << " " << v << endl;
        int x;
        cin >> x;
        return x;
    };

    int m, n;
    cin >> m >> n;
    for (int i = 1; i < m; i++) {
        cout << i << " " << i + 1 << endl;
    }

    int ans = m - 1;
    for (int i = 1; i <= 100; i++) {
        while (true) {
            int u = rnd() % n + 1;
            int v = rnd() % n + 1;
            if (u != v) {
                ans = max(ans, subtree(u, v) + subtree(v, u) - dist(u, v));
                break;
            }
        }
    }
    cout << "! " << ans << endl;

    return 0;
}
```