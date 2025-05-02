```cpp
#include<bits/stdc++.h>
using namespace std;
 
#ifdef lCAL
#include "debug.h"
#else
#define debug(...) 42
#endif
 
const int MAX_N = 2e5 + 1;
 
int st[MAX_N * 2];
int depth[MAX_N], pos[MAX_N], val[MAX_N], sz[MAX_N], par[MAX_N],
    chain_id[MAX_N];
vector<int> adj[MAX_N];
int n, q;
 
void update(int idx, int v) {
	st[idx += n] = v;
	for (idx /= 2; idx; idx /= 2) {
        st[idx] = st[2 * idx] ^ st[2 * idx + 1];
    }
}
 
int query(int l, int r) {
	int ra = 0, rb = 0;
	for (l += n, r += n + 1; l < r; l /= 2, r /= 2) {
		if (l & 1) ra = ra ^ st[l++];
		if (r & 1) rb = rb ^ st[--r];
	}
	return ra ^ rb;
}
 
void dfs(int u, int p) {
	sz[u] = 1;
	par[u] = p;
	for (int v : adj[u]) {
		if (v == p) continue;
		depth[v] = depth[u] + 1;
        dfs(v, u);
        sz[u] += sz[v];
	}
}
int cur_pos = 1;
void hld(int u, int par, int top) {
	pos[u] = cur_pos++;
	chain_id[u] = top;
	update(pos[u], val[u]);
	int h_v = -1, h_sz = -1;
	for (int v : adj[u]) {
		if (v == par) continue;
		if (sz[v] > h_sz) {
			h_sz = sz[v];
			h_v = v;
		}
	}
	if (h_v == -1) return;
	hld(h_v, u, top);
	for (int v : adj[u]) {
		if (v == par || v == h_v) continue;
		hld(v, u, v);
	}
}
 
int path(int x, int y) {
	int ret = 0;
	while (chain_id[x] != chain_id[y]) {
		if (depth[chain_id[x]] < depth[chain_id[y]]) swap(x, y);
		ret ^= query(pos[chain_id[x]], pos[x]);
		x = par[chain_id[x]];
	}
	if (depth[x] > depth[y]) swap(x, y);
	ret ^= query(pos[x], pos[y]);
	return ret;
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);
    
    // freopen("cowland.in", "r", stdin);
    // freopen("cowla %%nd.out", "w", stdout);
    cin >> n >> q;
    for (int i = 1; i <= n; i++) {
        cin >> val[i];
    }
    for (int i = 1; i < n; i++) {
        int u, v;   cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    dfs(1, 1);
    hld(1, 1, 1);
    while (q--) {
        int t;  cin >> t;
        if (t == 1) {
            int s, x;   cin >> s >> x;
            val[s] = x;
            update(pos[s], x);
        }
        else {
            int a, b;   cin >> a >> b;
            cout << path(a, b) << '\n';
        }
    }   
 
    return 0;
}
```