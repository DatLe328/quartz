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
        sort(a.begin(), a.end());
        map<int, int> mp;   // Count number
        int best = 1;       // Answer
        int dict = 0;       // Distinct value
        deque<int> dq;
        for (int i = 0; i < n; i++) {
            if (dq.size() && a[i] - a[dq.back()] > 1) {
                dq.clear();
                dict = 0;
            }
            if (mp[a[i]] == 0) {
                dict++;
            }
            mp[a[i]]++;
            dq.push_back(i);
            while (dict > k) {
                if (mp[a[dq.front()]] == 1) {
                    dict--;
                }
                mp[a[dq.front()]]--;
                dq.pop_front();
            }
            best = max(best, i - dq.front() + 1);
        }
        cout << best << '\n';
    }

    return 0;
}
```