```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 2e5 + 1;
ll st[10][MAX_N * 4];
int a[MAX_N];

int calc(int i, int k) {
    if (i % k == 0) return 2;
    while (i) {
        if (i % 10 == k) return 2;
        i /= 10;
    }
    return 1;
}
void build(int k, int id, int l, int r) {
    if (l == r) {
        st[k][id] = a[l] * calc(l, k);
        return;
    }
    int mid = (l + r) / 2;
    build(k, id * 2, l, mid);
    build(k, id * 2 + 1, mid + 1, r);
    st[k][id] = st[k][id * 2] + st[k][id * 2 + 1];
}
void update(int k, int id, int l, int r, int i, int c) {
    if (l > i || r < i) return;
    if (l == r) {
        st[k][id] = c * calc(i, k);
        return;
    }
    int mid = (l + r) / 2;
    update(k, id * 2, l, mid, i, c);
    update(k, id * 2 + 1, mid + 1, r, i, c);
    st[k][id] = st[k][id * 2] + st[k][id * 2 + 1];
}
ll query(int k, int id, int l, int r, int u, int v) {
    if (l > v || r < u) return 0;
    if (l >= u && r <= v) {
        return st[k][id];
    }
    int mid = (l + r) / 2;
    return query(k, id * 2, l, mid, u, v) + query(k, id * 2 + 1, mid + 1, r, u, v);
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n, q;   cin >> n >> q;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    for (int i = 1; i <= 9; i++) build(i, 1, 1, n);
    while (q--) {
        int t;  cin >> t;
        if (t == 1) {
            int i, c;   cin >> i >> c;
            for (int k = 1; k <= 9; k++) {
                update(k, 1, 1, n, i, c);
            }
        }
        else {
            int l, r, k;    cin >> l >> r >> k;
            cout << query(k, 1, 1, n, l, r) << '\n';
        }
    }   

    return 0;
}
```