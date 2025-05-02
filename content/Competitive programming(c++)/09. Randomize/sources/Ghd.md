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

int rand(int l, int r) {
    return uniform_int_distribution<int>(l, r)(rng);
}

const int MAX_N = 1e6;
ll a[MAX_N], d[MAX_N], cnt[MAX_N];
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n;  cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    ll ans = 1;
    for (int i = 0; i < 10; i++) {
        // ll current = chrono::steady_clock().now().time_since_epoch().count();
        // if (current - start > 3 * 1e9) break;
        ll res = a[rand(0, n - 1)];
        int idx = 0;
        for (ll i = 1; i * i <= res; i++) {
            if (res % i == 0) {
                d[idx++] = i;
                if (i * i != res) {
                    d[idx++] = res / i;
                }
            }
        }

        for (int i = 0; i < idx; i++) {
            cnt[i] = 0;
        }
        sort(d, d + idx);
        for (int i = 0; i < n; i++) {
            ll g = __gcd(res, a[i]);
            cnt[lower_bound(d, d + idx, g) - d]++;
        }
        for (int i = 0; i < idx; i++) {
            for (int j = i + 1; j < idx; j++) {
                if (d[j] % d[i] == 0) {
                    cnt[i] += cnt[j];
                }
            }
            if (2 * cnt[i] >= n && d[i] > ans) {
                ans = d[i];
            }
        }
    }
    cout << ans;

    return 0;
}

```