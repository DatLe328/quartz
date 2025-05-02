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

    int n, m, k;    cin >> n >> m >> k;
    vector<vector<char>> a(n, vector<char>(m));
    vector<vector<int>> enemy(n);
    vector<vector<int>> pref_walls(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> a[i][j];
            if (a[i][j] == 'x') {
                enemy[i].push_back(j);
            }
            if (a[i][j] == '#') {
                pref_walls[i][j] = 1;
            }
        }
        for (int j = 1; j < m; j++) {
            pref_walls[i][j] += pref_walls[i][j - 1];
        }
    }
    auto get_walls = [&] (int row, int l, int r) -> int {
        if (l == 0) return pref_walls[row][r];
        return pref_walls[row][r] - pref_walls[row][l - 1];
    };
    auto valid = [&] (int v) -> bool {
        int need = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < (int)enemy[i].size(); j++) {
                int pos = enemy[i][j];
                need++;
                int center_boom = pos;
                for (int p = pos; p <= min(m - 1, pos + v); p++) {
                    if (a[i][p] == '#') break;
                    center_boom = p;
                }
                int max_right = min(center_boom + v, m - 1);
                int tmp = j;
                for (int t = j + 1; t < (int)enemy[i].size(); t++) {
                    if (enemy[i][t] <= max_right && get_walls(i, pos, enemy[i][t]) == 0) {
                        tmp = t;
                    }
                    else {
                        break;
                    }
                }
                j = tmp;
            }
        }
        return need <= k;
    };
    int l = 0, r = m / 2 + 1;
    int ans = -1;
    while (l <= r) {
        int mid = (l + r) / 2;
        if (valid(mid)) {
            ans = mid;
            r = mid - 1;
        }
        else {
            l = mid + 1;
        }
    }
    cout << ans;

    return 0;
}
```