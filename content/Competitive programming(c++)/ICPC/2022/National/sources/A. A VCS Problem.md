```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

struct DSU {
    int n;
    vector<int> par, sz;
    DSU(int n) : par(n + 1, -1), sz(n + 1, 1) {}
    
    int find(int u) {
        return par[u] == -1 ? u : par[u] = find(par[u]);
    }
    void unite(int u, int v) {
        u = find(u);
        v = find(v);
        if (u == v) return;
        if (sz[u] < sz[v]) swap(u, v);
        par[v] = u;
        sz[u] += sz[v];
    }
};

struct Query {
    int id, s, u, v;
    Query(int id, int s, int u, int v) : id(id), s(s), u(u), v(v) {}

    int operator< (const Query& b) {
        Query a = *this;
        return a.s < b.s;
    }
};
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n, q;   cin >> n >> q;
    DSU dsu(n);
    vector<pair<int, int>> edgs;
    vector<int> check_point = {0};
    vector<Query> qs;
    for (int i = 0; i < q; i++) {
        char c; cin >> c;
        if (c == 'A') {
            int u, v;   cin >> u >> v;
            edgs.push_back({u, v});
        }
        if (c == '?') {
            int u, v;   cin >> u >> v;
            qs.push_back(Query(qs.size(), edgs.size(), u, v));
        }
        if (c == 'C') {
            check_point.push_back(edgs.size());
        }
        if (c == 'Q') {
            int s, u, v;    cin >> s >> u >> v;
            qs.push_back(Query(qs.size(), check_point[s], u, v));
        }
    }
    vector<int> ans(qs.size());
    sort(qs.begin(), qs.end());
    int cur_l = 0;
    for (auto p : qs) {
        while (cur_l < p.s) {
            dsu.unite(edgs[cur_l].first, edgs[cur_l].second);
            cur_l++;
        }
        ans[p.id] = dsu.find(p.u) != dsu.find(p.v);
    }
    for (int x : ans) {
        cout << (x ? "N" : "Y");
    }

    return 0;
}
```