```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

ll start = chrono::steady_clock().now().time_since_epoch().count();
mt19937_64 rng(start);

const int MAX_N = 51;
bool a[MAX_N][MAX_N], base[MAX_N][MAX_N];
int cost[MAX_N * 2];
int n, ans;
bitset<MAX_N * 2> cur, res;

void opt() {
    for (int i = 0; i < n; i++) {
        if (cur[i]) {
            for (int j = 0; j < n; j++) {
                a[i][j] ^= 1;
            }
        }
    }
    for (int i = n; i < 2 * n; i++) {
        if (cur[i]) {
            for (int j = 0; j < n; j++) {
                a[j][i - n] ^= 1;
            }
        }
    }
    int total = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            int val = (a[i][j] ? -1 : 1);
            cost[i] += val;
            cost[j + n] += val;
            total += a[i][j];
        }
    }
    while (true) {
        int u = -1, mn = 0;
        for (int i = 0; i < 2 * n; i++) {
            if (cost[i] < mn) {
                mn = cost[i];
                u = i;
            }
        }
        if (u == -1) break;
        cur[u] = cur[u] ^ 1;
        if (u < n) {
            for (int j = 0; j < n; j++){
                a[u][j] ^= 1;
                cost[j + n] += (a[u][j] ? -2 : 2);
            }
        }
        else {
            for (int i = 0; i < n; i++) {
                a[i][u - n] ^= 1;
                cost[i] += (a[i][u - n] ? -2 : 2);
            }
        }
        total += cost[u];
        cost[u] = n - cost[u];
    }
    if (total < ans) {
        ans = total;
        res = cur;
    }
}

int rand_int(int l, int r) {
    return uniform_int_distribution<int>(l, r)(rng);
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> base[i][j];
            a[i][j] = base[i][j];
            ans += base[i][j];
        }
    }
    opt();
    while (true) {
        ll current = chrono::steady_clock().now().time_since_epoch().count();
        if (current - start > 0.9 * 1e9) {
            break;
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                a[i][j] = base[i][j];
            }
        }
        for (int i = 0; i < 2 * n; i++) {
            cur[i] = rand_int(0, 1);
            cost[i] = 0;
        }
        opt();
    }
    cout << ans << '\n';
    for (int i = 0; i < 2 * n; i++) {
        cout << res[i] << '\n';
    }
    return 0;
}
```