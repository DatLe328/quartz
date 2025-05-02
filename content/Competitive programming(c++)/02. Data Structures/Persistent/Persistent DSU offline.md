```cpp
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
        par.resize(n);
        rank.resize(n);
        for (int i = 0; i < n; i++) {
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

    void rollback() {
        int target = snap.top();    snap.pop();
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
```