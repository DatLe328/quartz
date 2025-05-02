The permutation is random so we just simply use binary search to ask $mid$ and $mid + 1$, if $a[mid + 1] > a[mid]$ we continue to ask $[l, mid]$, then we need to check the answer
```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

int main() {
    int n;  cin >> n;
    vector<int> p(n + 2, -1);
    p[0] = p[n + 1] = INT_MAX;
    int l = 1, r = n;
    while (l <= r) {
        int mid = (l + r) / 2;
        if (p[mid] == -1) {
            cout << "? " << mid << endl;
            cin >> p[mid];
        }
        if (p[mid + 1] == -1) {
            cout << "? " << mid + 1 << endl;
            cin >> p[mid + 1];
        }
        for (int i = max(1, mid - 1); i <= min(n, mid + 1); i++) {
            if (p[i] == -1) continue;
            int cnt = 0;
            for (int j = i - 1; j <= i + 1; j++) {
                if (p[j] != -1 && p[j] > p[i]) {
                    cnt++;
                }
            }
            if (cnt == 2) {
                cout << "! " << i << endl;
                return 0;
            }
        }
        if (p[mid] < p[mid + 1]) {
            r = mid;
        }
        else {
            l = mid;
        }
    }
    
    

    return 0;
}
```