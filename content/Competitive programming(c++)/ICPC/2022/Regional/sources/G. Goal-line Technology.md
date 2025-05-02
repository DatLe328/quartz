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

    int n, r, x1, x2;   cin >> n >> r >> x1 >> x2;
    bool goal = false;
    for (int i = 0; i < n; i++) {
        int center; cin >> center;
        if (x1 > x2) {
            if (center - r >= x1) {
                goal = true;
                break;
            }
        }
        else {
            if (center + r <= x1) {
                goal = true;
                break;
            }
        }
    }
    cout << (goal ? "GOAL\n" : "NO GOAL\n");

    return 0;
}
```