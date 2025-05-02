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
vector<pair<int, int>> edges[MAX_N * 2];
int deg[MAX_N], vis[2 * MAX_N];
int n, m;
vector<int> trace;

/*
    we don't use the below dfs because vis[idx]
    only use to mark visited edge not vertex so
    when we revisit each vertex that we cause 
    unnecessaries loops with visted edge and we
    will get time limit exceeded

    void dfs(int u) {
        for (auto [v, idx] : adj[u]) {
            if (!vis[idx]) {
                vis[idx] = 1;
                dfs(v);
            }
        }
        trace.push_back(u);
    }
*/

/*
	You may wonder why the trace vector is put in the end
	of dfs function because no matter you go backward or 
	forward it still an euler because this is undirected
	graph.
	Important: trace vector it must place the end or it 
	give wrong answer
*/
void dfs(int u) {
    while (adj[u].size()) {
        auto [v, idx] = adj[u].back();
        adj[u].pop_back();
        if (!vis[idx]) {
            vis[idx] = 1;
            dfs(v);
        }
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
        adj[v].push_back({u, i});
        deg[u]++;
        deg[v]++;
    }  
    // Check if have an odd deg vertex, we won't have euler tour
    for (int i = 1; i <= n; i++) {
        if (deg[i] & 1) {
            cout << "IMPOSSIBLE\n";
            exit(0);
        }
    }
    dfs(1);
    // Check if euler tour have visted all edges
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