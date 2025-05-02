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
        int a, b, c;    cin >> a >> b >> c;
        if (c > 0) {
            cout << "CURVE UP\n";
        }
        else if (c < 0) {
            cout << "CURVE DOWN\n";
        }
        else {
            cout << "NO CURVE\n";
        }
    }

    return 0;
}
```