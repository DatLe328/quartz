# Solution 1: Use Mo's Algorithm and Euler tour
The main idea is use Euler tour to have query's segment then solve the problem like [[Distinct Values Queries]]
```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 2e5 + 1;
const int d = 500;
vector<int> adj[MAX_N];
int color[MAX_N], n, timer = 0, num[MAX_N], tail[MAX_N], id[MAX_N], ans[MAX_N], cnt[MAX_N];
array<int, 3> query[MAX_N];
int dict = 0;

void dfs(int u, int p) {
    num[u] = tail[u] = ++timer;
    id[num[u]] = u;
    for (int v : adj[u]) {
        if (v == p) continue;
        if (!num[v]) {
            dfs(v, u);
        }
    }
    tail[u] = timer;
}

int cmp(array<int, 3> a, array<int, 3> b) {
    return make_pair(a[0] / d, a[1]) < make_pair(b[0] / d, b[1]);
}
void add(int idx) {
    if (cnt[color[id[idx]]] == 0) {
        dict++;
    }
    cnt[color[id[idx]]]++;
}

void remove(int idx) {
    if (cnt[color[id[idx]]] == 1) {
        dict--;
    }
    cnt[color[id[idx]]]--;
}

void compress() {
    map<int, int> mp;
    for (int i = 1; i <= n; i++) {
        mp[color[i]] = i;
    }
    for (int i = 1; i <= n; i++) {
        color[i] = mp[color[i]];
    }
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> color[i];
    }
    compress();
    for (int i = 0; i < n - 1; i++) {
        int u, v;   cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    dfs(1, 1);
    for (int i = 1; i <= n; i++) {
        query[i] = {num[i], tail[i], i};
    }
    sort(query + 1, query + 1 + n, cmp);
    int curL = 1, curR = 1;
    add(1);
    for (int i = 1; i <= n; i++) {
        auto [l, r, idx] = query[i];
        while (curL < l) remove(curL++);
        while (curL > l) add(--curL);
        while (curR < r) add(++curR);
        while (curR > r) remove(curR--);
       
        ans[idx] = dict;
    }
    for (int i = 1; i <= n; i++) {
        cout << ans[i] << ' ';
    }

    return 0;
}

```
# Solution 2: Small-To-Large Merging
**Just use set for each vertex and apply [[Small-To-Large Merging]]**, *remember to assign answer for each node immediately because it will swap from leaf to root, if we don't assign answer, except the first answer will always 1 distinct color*
```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 2e5 + 1;
int n, ans[MAX_N];
vector<int> adj[MAX_N];
set<int> color[MAX_N];

void dfs(int u, int p) {
    for (int v : adj[u]) {
        if (v == p) continue;
        dfs(v, u);
        if (color[v].size() > color[u].size()) {
            swap(color[v], color[u]);
        }
        for (int i : color[v]) {
            color[u].insert(i);
        }
    }
    ans[u] = color[u].size();
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n;
    for (int i = 1; i <= n; i++) {
        int x;  cin >> x;
        color[i].insert(x);
    }
    for (int i = 0; i < n - 1; i++) {
        int u, v;   cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    dfs(1, 1);
    for (int i = 1; i <= n; i++) {
        cout << ans[i] << ' ';
    }

    return 0;
}

```