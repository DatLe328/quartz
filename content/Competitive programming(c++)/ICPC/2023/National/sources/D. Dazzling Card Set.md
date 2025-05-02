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
    while(tt--) {
        int n;  cin >> n;
        vector<int> a(n);
        for (auto &i : a) {
            cin >> i;
        }
        multiset<int> ms;
        long long ans = 0;
        int cur_l = 0, cur_r = -1;
        for (int i = 0; i < n; i++) {
            while (cur_l <= i && ms.find(a[i]) != ms.end()) {
                ms.erase(ms.find(a[cur_l]));
                cur_l++;
            }
            while (cur_r < i && ms.find(a[cur_r + 1]) == ms.end()) {
                cur_r++;
                ms.insert(a[cur_r]);
            }
            ans += cur_r - cur_l + 1;
        }
        cout << ans << '\n';
    }

    return 0;
}
```