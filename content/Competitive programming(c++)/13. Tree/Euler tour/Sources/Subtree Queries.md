```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

struct FenwickTree {
    int n;
    vector<ll> bit, arr;
    FenwickTree(int n) : n(n), bit(n + 1, 0), arr(n + 1) {}
    void update(int i, int val) {
        arr[i] += val;
        while (i <= n) {
            bit[i] += val;
            i += i & (-i);
        }
    }
    void set(int i, int val) {
        update(i, val - arr[i]);
    }
    ll pref(int i) {
        ll ret = 0;
        while (i) {
            ret += bit[i];
            i -= i & (-i);
        }
        return ret;
    }
};

const int MAX_N = 2e5 + 1;
vector<int> adj[MAX_N];
int n, q, num[MAX_N], tail[MAX_N], timer = 0, a[MAX_N];

void euler_tour(int u, int p) {
    num[u] = tail[u] = ++timer;
    for (int v : adj[u]) {
        if (v == p) continue;
        if (!num[v]) {
            euler_tour(v, u);
        }
    }
    tail[u] = timer;
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> q;
    FenwickTree fen(n);
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    for (int i = 0; i < n - 1; i++) {
        int u, v;   cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    euler_tour(1, 1);
    for (int i = 1; i <= n; i++) {
        fen.update(num[i], a[i]);
    }
    while (q--) {
        int t;  cin >> t;
        if (t == 1) {
            int i, val; cin >> i >> val;
            fen.set(num[i], val);
        }
        else {
            int s;  cin >> s;
            ll pref_end = fen.pref(tail[s]);
            ll pref_start = fen.pref(num[s] - 1);
            cout << pref_end - pref_start << '\n';
        }
    }
    return 0;
}

```