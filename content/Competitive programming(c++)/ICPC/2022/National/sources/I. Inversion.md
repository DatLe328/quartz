```cpp
#include <bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

int main()
{
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n;
    cin >> n;
    vector<vector<int>> a(n, vector<int>(n));
    vector<int> cnt(n);
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> a[i][j];
        }
    }
    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            if (a[i][j] > a[i][i])
                cnt[j]++;
            else
                cnt[i]++;
        }
    }
    for (int i = 0; i < n; i++)
        cout << cnt[i] + 1 << ' ';

    return 0;
}
```