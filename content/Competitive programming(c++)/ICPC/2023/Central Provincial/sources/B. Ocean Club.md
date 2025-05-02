![[2023_Central_B.png]]
```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 101;
int a[MAX_N];
vector<int> adj[MAX_N];
int dp[MAX_N][MAX_N][MAX_N];
int p[MAX_N];

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n;  cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    sort(a + 1, a + 1 + n);
    for (int i = 1; i <= n; i++) {
        p[a[i]] = i;
    }
    for (int i = 1; i <= n; i++) {
        for (int j = i + 1; j <= n; j++) {
            if (__gcd(a[i], a[j]) == 1) {
                adj[i].push_back(j);
            }
        }
    }
    for (int i = 1; i <= n; i++) {
        dp[i][i][0] = 1;
    }
    for (int k = 0; k < n; k++) {
        for (int u = 1; u <= n; u++) {
            for (int v = u; v <= n; v++) {
                if (dp[u][v][k]) {
                    for (int nv : adj[v]) {
                        dp[u][nv][k + 1] = (dp[u][nv][k + 1] + dp[u][v][k]) % 2023;
                    }
                }
            }
        }
    }
    int q;  cin >> q;
    while (q--) {
        int l, r, k;    cin >> l >> r >> k;
        int u = p[l];
        int v = p[r];
        cout << dp[u][v][k] << '\n';
    }

    return 0;
}
```