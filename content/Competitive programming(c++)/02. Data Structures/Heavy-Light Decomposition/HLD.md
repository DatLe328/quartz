#tree #graphs
```cpp
const int MAX_N = 2e5 + 1;

struct FenwickTree {
    int n;
    vector<int> bit;
    FenwickTree() {}
    FenwickTree(int n) : n(n), bit(n + 1) {}
    void update(int i, int v) {
        while (i <= n) {
            bit[i] += v;
            i += i & (-i);
        }
    }
    void range_update(int l, int r, int v) {
        update(l, v);
        update(r + 1, -v);
    }
    int sum(int i) {
        int ret = 0;
        while (i) {
            ret += bit[i];
            i -= i & (-i);
        }
        return ret;
    }
} fen;

vector<int> adj[MAX_N];
int depth[MAX_N], par[MAX_N], sub[MAX_N];
int heavy[MAX_N], head[MAX_N], pos[MAX_N];
int cur_pos = 1;
int n, q;

int a[MAX_N], st[MAX_N * 2];

void update(int idx, int v) {
	st[idx += n] = v;
	for (idx /= 2; idx; idx /= 2) {
        st[idx] = max(st[2 * idx], st[2 * idx + 1]);
    }
}
 
int query(int l, int r) {
	int ra = 0, rb = 0;
	for (l += n, r += n + 1; l < r; l /= 2, r /= 2) {
		if (l & 1) ra = max(ra, st[l++]);
		if (r & 1) rb = max(rb, st[--r]);
	}
	return max(ra, rb);
}
void dfs(int u, int p) {
    depth[u] = depth[p] + 1;
    sub[u] = 1;
    par[u] = p;
    int max_sub = 0;
    for (int v : adj[u]) {
        if (v == p) continue;
        dfs(v, u);
        sub[u] += sub[v];
        if (sub[v] > max_sub) {
            max_sub = sub[v];
            heavy[u] = v;
        }
    }
}

void decompose(int u, int h) {
    head[u] = h;
    pos[u] = cur_pos++;
    if (heavy[u] != 0) {
        decompose(heavy[u], h);
    }
    for (int v : adj[u]){
        if (v != par[u] && v != heavy[u]) {
            decompose(v, v);
        }
    }
}
void update_path(int u, int v, int val) {
    while (head[u] != head[v]) {
        if (depth[head[u]] < depth[head[v]]) swap(u, v);
        fen.range_update(pos[head[u]], pos[u], val);
        u = par[head[u]];
    }
    if (depth[u] > depth[v]) swap(u, v);
    fen.range_update(pos[u], pos[v], val);
}

int query_path(int u, int v) {
    int ret = 0;
    while (head[u] != head[v]) {
        if (depth[head[u]] < depth[head[v]]) swap(u, v);
        int cur_max = query(pos[head[u]], pos[u]);
        ret = max(ret, cur_max);
        u = par[head[u]];
    }
	    if (depth[u] > depth[v]) swap(u, v);
    int last = query(pos[u], pos[v]);
    return max(ret, last);
}
void init() {
    fen = FenwickTree(n);
    dfs(1, 1);
    decompose(1, 1);
}
```