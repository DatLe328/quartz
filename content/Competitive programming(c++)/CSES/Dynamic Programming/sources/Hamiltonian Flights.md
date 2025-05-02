```cpp
#include<bits/stdc++.h>
using namespace std;
 
#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif
 
const int MAX_N = 20;
const int64_t MOD = 1e9 + 7;
vector<int> adj[MAX_N];
int64_t dp[1 << MAX_N][MAX_N];
 
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);
 
    int n, m;   cin >> n >> m;
    for (int i = 0; i < m; i++) {
        int u, v;   cin >> u >> v;
        u--, v--;
        adj[v].push_back(u);
    }
    dp[1][0] = 1;
    for (int mask = 2; mask < (1 << n); mask++) {
        if ((mask & (1 << 0)) == 0) continue;
		if ((mask & (1 << (n - 1))) && mask != ((1 << n) - 1)) continue;
        for (int u = 0; u < n; u++) {
            if (mask & (1 << u) == 0) {
                continue;
            }
            int prev = mask ^ (1 << u);
            for (int v : adj[u]) {
                if (mask & (1 << v)) {
                    dp[mask][u] += dp[prev][v];
                    dp[mask][u] %= MOD;
                }
            }
        }
    }
    cout << dp[(1 << n) - 1][n - 1];
 
    return 0;


```