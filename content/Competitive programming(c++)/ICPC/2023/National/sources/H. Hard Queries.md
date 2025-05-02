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
    vector<ll> bit;
    FenwickTree() : n(1), bit(1, 0) {}
    FenwickTree(int n) : n(n), bit(n + 1, 0) {}
    void update(int i, ll v) {
        while (i <= n) {
            bit[i] += v;
            i += i & (-i);
        }
    }
    ll query(int i) {
        ll ret = 0;
        while (i) {
            ret += bit[i];
            i -= i & (-i);
        }
        return ret;
    }
    ll range(int l, int r) {
        return query(r) - query(l - 1);
    }
};
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n, q;   cin >> n >> q;
    vector<int> a(n + 1);
    unordered_map<int, vector<int>> pos;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        pos[a[i]].push_back(i);
    }
    unordered_map<int, FenwickTree> fen;
    for (auto [i, v] : pos) {
        int sz = int(v.size());
        fen[i] = FenwickTree(sz);
        int cur = 1;
        for (int x : v) {
            fen[i].update(cur++, 1LL * x * x);
        }
    }

    auto index = [&] (int val, int p) -> int {
        return lower_bound(pos[val].begin(), pos[val].end(), p) - pos[val].begin();
    };
    auto update = [&] (int p, int c) -> int {
        int i = index(a[p], p); 
        fen[a[p]].update(i + 1, 1LL * c * p * p);
        return i;
    };
    ll last = 0;
    while (q--) {
        int t;  cin >> t;
        if (t == 1) {
            int p;  cin >> p;
            p = (p + last - 1) % (n - 1) + 1;
            if (a[p] != a[p + 1]) {
                int i = update(p, -1);
                int j = update(p + 1, -1);
                pos[a[p]][i] = p + 1;
                pos[a[p + 1]][j] = p;
                swap(a[p], a[p + 1]);
                update(p, 1);
                update(p + 1, 1);
            }
            continue;
        }
        int l, r, x;
        cin >> l >> r >> x;
        l = (l + last - 1) % n + 1;
        r = (r + last - 1) % n + 1;
        x = (x + last - 1) % n + 1;
        if (l > r) {
            swap(l, r);
        }
        if (t == 2) {
            last = fen[x].range(index(x, l) + 1, index(x, r + 1));
        }
        else {
            last = fen[x].range(l, r);
        }
        cout << last << '\n';
    }

    return 0;
}
```