# Tree Distances I
```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 2e5 + 1;
int n; 
int first[MAX_N];  // to store first-max length.
int second[MAX_N]; // to store second-max length.
int child[MAX_N];  // to store child for path of max length.
vector<int> adj[MAX_N];

// calculate for every node x the maximum
// length of a path that goes through a child of x
void dfs1(int u, int p) {
    first[u] = second[u] = 0;
    for (int v : adj[u]) {
        if (v == p) continue;
        dfs1(v, u);
        if (first[v] + 1 > first[u]) {
            second[u] = first[u];
            first[u] = first[v] + 1;
            child[u] = v;
        }
        else if (first[v] + 1 > second[u]) {
            second[u] = first[v] + 1;
        }
    }
}

// calculate for every node x the
// maximum length of a path through its parent p
void dfs2(int u, int p) {
    for (int v : adj[u]) {
        if (v == p) continue;
        if (child[u] == v) {
            if (first[v] < second[u] + 1) {
                second[v] = first[v];
                first[v] = second[u] + 1;
                child[v] = u;
            }
            else {
                second[v] = max(second[v], second[u] + 1);
            }
        }
        else {
            second[v] = first[v];
            first[v] = first[u] + 1;
            child[v] = u;
        }
        dfs2(v, u);
    }
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n;
    for (int i = 0; i < n - 1; i++) {
        int u, v;   cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    dfs1(1, 1);
    dfs2(1, 1);
    for (int i = 1; i <= n; i++) {
        cout << first[i] << ' ';
    }

    return 0;
}

```