# Solution 1: HLD
![[Pasted image 20241009212420.png]]
**Observation #1:** $dist(u,x)+dist(v,x)=dist(u,v)+2∗dist(x,y)$. In both sides of the equation, every edge in the heavy path appears once, and every edge in the path from $x$ to $y$ appears twice.

We know d$ist(u,x)=dep_x−dep_u$. We also know $dist(u,v)=dep_v−dep_u$. Additionally, we can query $dist(v,x)$. From these 3 pieces of information and the equation above, we can find $dist(x,y)$.

**Observation #2:** $dist(u,y)=dist(u,x)−dist(x,y)$. Now we know $dist(u,y)$. Since all nodes in the heavy path have different distances from node $u$, we know $y$ itself!
```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 2e5 + 1;
int sz[MAX_N], par[MAX_N], chain_id[MAX_N], depth[MAX_N];
vector<int> adj[MAX_N];
vector<int> chain_head;
int cur_chain = 1, depth_x, n;

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
void hld(int u, int p) {
    chain_head.push_back(u);
    int v_h = -1, h_sz = -1;
    for (int v : adj[u]) {
        if (v == p) continue;
        if (sz[v] > h_sz) {
            v_h = v;
            h_sz = sz[v];
        }
    }
    if (v_h != -1) {
        hld(v_h, u);
    }
}
int ask(char c, int u) {
    cout << c << ' ' << u << endl;
    int ret;    cin >> ret;
    return ret;
}
int solve(int u) {
    chain_head.clear();
    hld(u, par[u]);
    int depth_y = (depth_x + depth[chain_head.back()] - ask('d', chain_head.back())) / 2;
    int y = chain_head[depth_y - depth[u]];
    if (depth_x == depth_y) {
        return y;
    }
    return solve(ask('s', y));
}
int main() {
    cin >> n;
    for (int i = 1; i < n; i++) {
        int u, v;   cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    dfs(1, 1);
    depth_x = ask('d', 1);
    int ans = solve(1);
    cout << "! " << ans << endl;

    return 0;
}
```