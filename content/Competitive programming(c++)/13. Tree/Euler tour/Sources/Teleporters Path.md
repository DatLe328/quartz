```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 1e5 + 1;
vector<pair<int, int>> adj[MAX_N];
int in[MAX_N], out[MAX_N];
int n, m;
vector<int> trace;

void dfs(int u) {
    while (adj[u].size()) {
        auto [v, idx] = adj[u].back();
        adj[u].pop_back();
        dfs(v);
    }
    trace.push_back(u);
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> m;
    for (int i = 1; i <= m; i++) {
        int u, v;   cin >> u >> v;
        adj[u].push_back({v, i});
        in[v]++;
        out[u]++;
    }
    
    if (in[1] + 1 != out[1] || in[n] != out[n] + 1) {
        cout << "IMPOSSIBLE\n";
        exit(0);
    }
    for (int i = 2; i < n; i++) {
        if (in[i] != out[i]) {
            cout << "IMPOSSIBLE\n";
            exit(0);
        }
    }

    dfs(1);
    reverse(trace.begin(), trace.end());
    if (trace.size() != m + 1) {
        cout << "IMPOSSIBLE\n";
    }
    else {
        for (int i : trace) {
            cout << i << ' ';
        }
    }

    return 0;
}

```