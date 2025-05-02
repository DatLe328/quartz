```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
 
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);
 
    if (fopen("A_input.txt", "r"))
        freopen("A_input.txt", "r", stdin);
    int n, m, q;    cin >> n >> m >> q;
    vector<vector<ll>> dist(n + 1, vector<ll>(n + 1, 1e18));
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            if (i == j)
                dist[i][j] = 0;
        }
    }
    for (int i = 0; i < m; i++) {
        int u, v, w;    cin >> u >> v >> w;
        dist[u][v] = dist[v][u] = min(dist[u][v], (ll)w);
    }
    for (int k = 1; k <= n; k++) {
        for (int u = 1; u <= n; u++) {
            for (int v = 1; v <= n; v++) {
                dist[u][v] = min(dist[u][v], dist[u][k] + dist[k][v]);
            }
        }
    }
    while (q--) {
        int u, v;   cin >> u >> v;
        if (dist[u][v] == (ll)1e18) {
            cout << -1 << '\n';
        }
        else {
            cout << dist[u][v] << '\n';
        }
    }
    return 0;
}

```