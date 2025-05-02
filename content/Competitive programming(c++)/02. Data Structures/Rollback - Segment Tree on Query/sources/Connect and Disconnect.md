```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

struct dsu_save {
    int v, rank_v, u, rank_u;
};
struct dsu_with_rollbacks {
    vector<int> par, rank;
    int comps;
    stack<dsu_save> op;
    stack<int> snap;

    dsu_with_rollbacks() {}

    dsu_with_rollbacks(int n) {
        par.resize(n + 1);
        rank.resize(n + 1);
        for (int i = 1; i <= n; i++) {
            par[i] = i;
            rank[i] = 0;
        }
        comps = n;
    }

    int find(int v) {
        return (v == par[v]) ? v : find(par[v]);
    }
    void persist() {
        snap.push(op.size());
    }
    bool unite(int v, int u) {
        v = find(v);
        u = find(u);
        if (v == u)
            return false;
        comps--;
        if (rank[v] > rank[u])
            swap(v, u);
        op.push({v, rank[v], u, rank[u]});
        par[v] = u;
        if (rank[u] == rank[v])
            rank[u]++;
        return true;
    }

    void rollback(int target) {
        while (op.size() > target) {
            dsu_save x = op.top();
            op.pop();
            comps++;
            par[x.v] = x.v;
            rank[x.v] = x.rank_v;
            par[x.u] = x.u;
            rank[x.u] = x.rank_u;
        }
    }
};

const int MAX_N = 3e5 + 1;
vector<pair<int, int>> st[MAX_N * 4];
dsu_with_rollbacks dsu;
map<pair<int, int>, int> pos;
int ans[MAX_N];
int n, q;

void update(int id, int l, int r, int u, int v, pair<int, int> k) {
    if (l > v || r < u) return;
    if (l >= u && r <= v) {
        st[id].push_back(k);
        return;
    }
    int mid = (l + r) / 2;
    update(id * 2, l, mid, u, v, k);
    update(id * 2 + 1, mid + 1, r, u, v, k);
}
void dfs(int id, int l, int r) {
    if (l > r) return;
    int until = dsu.op.size();
    for (auto [u, v] : st[id]) {
        dsu.unite(u, v);
    }
    if (l == r) {
        ans[l] = dsu.comps;
        dsu.rollback(until);
        return;
    }
    int mid = (l + r) / 2;
    dfs(id * 2, l, mid);
    dfs(id * 2 + 1, mid + 1, r);
    dsu.rollback(until);
}

void add_edge(int id, int u, int v) {
    if (u > v) swap(u, v);
    pos[{u, v}] = id;
}

void remove_edge(int id, int u, int v) {
    if (u > v) swap(u, v);
    update(1, 1, q, pos[{u, v}], id, {u, v});
    pos.erase({u, v});
}

void solve() {
    for (auto [u, v] : pos) {
        update(1, 1, q, v, q, u);
    }
    dfs(1, 1, q);
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> q;
    dsu = dsu_with_rollbacks(n);
    vector<int> que;
    for (int i = 1; i <= q; i++) {
        char c; cin >> c;
        if (c == '?') {
            que.push_back(i);
        }
        else if (c == '+') {
            int u, v;   cin >> u >> v;
            add_edge(i, u, v);
        }
        else {
            int u, v;   cin >> u >> v;
            remove_edge(i, u, v);
        }
    }
    
    solve();

    for (auto x : que) {
        cout << ans[x] << '\n';
    }

    return 0;
}
```