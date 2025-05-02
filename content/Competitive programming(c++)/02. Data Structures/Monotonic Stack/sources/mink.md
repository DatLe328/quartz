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
        int n, k;   cin >> n >> k;
        vector<int> a(n);
        for (auto &i : a) {
            cin >> i;
        }
        deque<int> dq;

        for (int i = 0; i < n; i++) {
            while (dq.size() && a[dq.back()] >= a[i]) {
                dq.pop_back();
            }
            dq.push_back(i);
            if (i >= k - 1) {
                cout << a[dq.front()] << ' ';
            }
            if (i - dq.front() + 1 >= k) {
                dq.pop_front();
            }
        }
        cout << '\n';
    }

    return 0;
}

```