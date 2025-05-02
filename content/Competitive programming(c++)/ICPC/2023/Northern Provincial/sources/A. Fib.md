```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 100;
ll fib[MAX_N];
ll n;
int ans;

void solve(ll val, ll p) {
    if (val == 1) {
        ans++;
        return;
    }
    for (int i = p; i < MAX_N; i++) {
        if (fib[i] > val) return;
        if (val % fib[i] == 0) {
            solve(val / fib[i], i);
        }
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    fib[0] = 2;
    fib[1] = 3;
    for (int i = 2; i < MAX_N; i++) {
        fib[i] = fib[i - 1] + fib[i - 2];
        if (fib[i] >= 1e18) break;
    }
    int tt; cin >> tt;
    while (tt--) {
        cin >> n;
        ans = 0;
        solve(n, 0);
        cout << ans << '\n';
    }

    return 0;
}
```