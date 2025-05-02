# LCA Binary lifting

**Binary Lifting:** Chuẩn bị trước bằng cách lưu trữ tổ tiên của mỗi nút ở các mức độ khác nhau: 2^0, 2^1, ..., 2^t với t vào khoảng log(n)

## Độ phức tạp

- **Thời gian**: O(n log⁡ n) cho tiền xử lý và O(log⁡ n) cho mỗi truy vấn.
- **Không gian**: O(n log n)
```cpp
// https://www.spoj.com/problems/LCA/
struct LCA {
    int n, l, timer;
    vector<int> tin, tout;
    vector<vector<int>> adj, up;
    LCA() {
        cin >> n;
        adj.resize(n + 1);
        tin.resize(n + 1);
        tout.resize(n + 1);
        l = ceil(log2(n));
        up.assign(n + 1, vector<int>(l + 1));
        timer = 0;
        for (int parent_node = 1; parent_node <= n; parent_node++) {
            int m;  cin >> m;
            for (int child_node = 1; child_node <= m; child_node++) {
                int child;  cin >> child;
                adj[parent_node].push_back(child);
                adj[child].push_back(parent_node);
            }
        }
        dfs(1, 1);
    }

    void dfs(int v, int p)
    {
        tin[v] = ++timer;
        up[v][0] = p;
        for (int i = 1; i <= l; ++i)
            up[v][i] = up[up[v][i-1]][i-1];

        for (int u : adj[v]) {
            if (u != p)
                dfs(u, v);
        }

        tout[v] = ++timer;
    }

    bool is_ancestor(int u, int v)
    {
        return tin[u] <= tin[v] && tout[u] >= tout[v];
    }

    int lca(int u, int v)
    {
        if (is_ancestor(u, v))
            return u;
        if (is_ancestor(v, u))
            return v;
        for (int i = l; i >= 0; --i) {
            if (!is_ancestor(up[u][i], v))
                u = up[u][i];
        }
        return up[u][0];
    }
};
```

```cpp
#include<bits/stdc++.h>
#define pii pair<int, int>
using namespace std;

const int MAX_N = 1e5 + 1;
const int MAX_L = 20;

int par[MAX_N][MAX_L], dep[MAX_N], dist[MAX_N][MAX_L];
vector<pii> adj[MAX_N];
void dfs(int u, int p = 0) {
	par[u][0] = p;
	for (int i = 1; i < MAX_L; i++) {
		par[u][i] = par[par[u][i - 1]][i - 1];
		if (par[u][i] != 0)
			dist[u][i] = dist[par[u][i - 1]][i - 1] + dist[u][i - 1];
	}
	for (auto [v, w]: adj[u]) {
		if (v == p)	continue;
		dist[v][0] = w;
		dep[v] = dep[u] + 1;
		dfs(v, u);
	}
}

int ancestor(int u, int k, long long &res) {
	for (int i = 0; i < MAX_L; i++)
		if (k & (1 << i)) {
			if (i == 0)
				res += dist[u][i];
			else
				res += dist[par[u][i - 1]][i - 1] + dist[u][i - 1];
			// cout << "res " << res << '\n';
			u = par[u][i];
		}
	return u;
}

long long lca(int u, int v) {
	long long res = 0;
	if (dep[u] < dep[v])
		swap(u, v);
	u = ancestor(u, dep[u] - dep[v], res);
	if (u == v)
		return res;
	
	for (int i = MAX_L - 1; i >= 0; i--)
		if (par[u][i] != par[v][i]) {
			res += dist[u][i] + dist[v][i];
			u = par[u][i], v = par[v][i];
		}
	res += dist[u][0] + dist[v][0];
	return res;
}

void solve() {
	int n, q;	cin >> n >> q;
	for (int i = 2; i <= n; i++) {
		int u, v, w;	cin >> u >> v >> w;
		adj[u].push_back({v, w});
		adj[v].push_back({u, w});
	}
	dfs(1);
	// cout << dist[4][1] << ' ' << dist[3][0] << '\n';
	while (q--) {
		int u, v;	cin >> u >> v;
		cout << lca(u, v) << '\n';
	}
}

int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

    if(fopen("B_input.txt", "r")) {
        freopen("B_input.txt", "r", stdin);
    }

	solve();
	return 0;
}

```