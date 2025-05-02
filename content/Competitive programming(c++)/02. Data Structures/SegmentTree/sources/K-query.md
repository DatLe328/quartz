```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 3e4 + 1;
int a[MAX_N];
vector<int> st[MAX_N * 4];

void build(int id, int l, int r) {
    if (l == r) {
        st[id].push_back(a[l]);
        return;
    }
    int mid = (l + r) / 2;
    build(id * 2, l, mid);
    build(id * 2 + 1, mid + 1, r);
    st[id].resize(st[id * 2].size() + st[id * 2 + 1].size());
    merge(st[id * 2].begin(), st[id * 2].end(), st[id * 2 + 1].begin(), st[id * 2 + 1].end(), st[id].begin());
}
int query(int id, int l, int r, int u, int v, int val) {
    if (l > v || r < u) {
        return 0;
    }
    // cout << l << ' ' << r << '\n';
    if (l >= u && r <= v) {
        return st[id].size() - (upper_bound(st[id].begin(), st[id].end(), val) - st[id].begin());
    }
    int mid = (l + r) / 2;
    auto left = query(id * 2, l, mid, u, v, val);
    auto right = query(id * 2 + 1, mid + 1, r, u, v, val);
    return left + right;
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n, q;   cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    build(1, 1, n);
    cin >> q;
    while (q--) {
        int l, r, k;    cin >> l >> r >> k;
        cout<< query(1, 1, n, l, r, k) << '\n';
    }

    return 0;
}

```