```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 2e5 + 1;

int par[MAX_N], depth[MAX_N], head[MAX_N], heavy[MAX_N], pos[MAX_N];
vector<int> adj[MAX_N];
int cur_pos = 1, n, q;
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

int dfs(int u, int p) {
    int sz = 1;
    int max_v_sz = 0;
    for (int v : adj[u]) {
        if (v == p) continue;
        par[v] = u;
        depth[v] = depth[u] + 1;
        int v_sz = dfs(v, u);
        sz += v_sz;
        if (v_sz > max_v_sz) {
            max_v_sz = v_sz;
            heavy[u] = v;
        }
    }
    return sz;
}
void decompose(int u, int h) {
    head[u] = h, pos[u] = cur_pos++;
    if (heavy[u] != 0) {
        decompose(heavy[u], h);
    }
    for (int v : adj[u]) {
        if (v != par[u] && v != heavy[u]) {
            decompose(v, v);
        }
    }
}
int path(int a, int b) {
    int res = 0;
    for (; head[a] != head[b]; b = par[head[b]]) {
        if (depth[head[a]] > depth[head[b]])
            swap(a, b);
        int cur_heavy_path_max = query(pos[head[b]], pos[b]);
        res = max(res, cur_heavy_path_max);
    }
    if (depth[a] > depth[b])
        swap(a, b);
    int last_heavy_path_max = query(pos[a], pos[b]);
    res = max(res, last_heavy_path_max);
    return res;
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> q;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    for (int i = 1; i < n; i++) {
        int u, v;   cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    dfs(1, 1);
    decompose(1, 1);
    for (int i = 1; i <= n; i++) {
        update(pos[i], a[i]);
    }
    while (q--) {
        int t;  cin >> t;
        if (t == 1) {
            int s, x;   cin >> s >> x;
            update(pos[s], x);
        }
        else {
            int a, b;   cin >> a >> b;
            cout << path(a, b) << ' ';
        }
    }   
    return 0;
}
```