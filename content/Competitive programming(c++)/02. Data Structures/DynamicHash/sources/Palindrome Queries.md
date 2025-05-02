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
const ll MOD = 1e9 + 7;
const int BASE = 128;

ll power[MAX_N];

void init() {
    power[0] = 1;
    for (int i = 1; i < MAX_N; i++) {
        power[i] = (power[i - 1] * BASE) % MOD;
    }
}
struct DynamicHash {
    vector<ll> st;
    string s;
    DynamicHash(string s) : s(s) {
        int n = (int)s.size();
        st.resize(4 * n);
        build(1, 1, n);
    }
    void build(int id, int l, int r) {
        if (l == r) {
            st[id] = s[l - 1];
            return;
        }
        int mid = (l + r) / 2;
        build(id * 2, l, mid);
        build(id * 2 + 1, mid + 1, r);
        st[id] = (st[id * 2] * power[r - mid] + st[id * 2 + 1]) % MOD;
    }
    void update(int id, int l, int r, int i, char c) {
        if (l > i || r < i) return;
        if (l == r) {
            st[id] = c;
            return;
        }
        int mid = (l + r) / 2;
        update(id * 2, l, mid, i, c);
        update(id * 2 + 1, mid + 1, r, i, c);
        st[id] = (st[id * 2] * power[r - mid] + st[id * 2 + 1]) % MOD;
    }
    pair<ll, int> query(int id, int l, int r, int u, int v) {
        if (l > v || r < u) return {0, 0};
        if (l >= u && r <= v) {
            return {st[id], r - l + 1};
        }
        int mid = (l + r) / 2;
        auto left = query(id * 2, l, mid, u, v);
        auto right = query(id * 2 + 1, mid + 1, r, u, v);
        ll ans = (left.first * power[right.second] + right.first) % MOD;
        return {ans, left.second + right.second};
    }
};

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    init();
    int n, q;   cin >> n >> q;
    string s;   cin >> s;
    DynamicHash dn1(s);
    reverse(s.begin(), s.end());
    DynamicHash dn2(s);
    while (q--) {
        int t;  cin >> t;
        if (t == 1) {
            int i;  cin >> i;
            char c; cin >> c;
            dn1.update(1, 1, n, i, c);
            dn2.update(1, 1, n, n - i + 1, c);
        }
        else {
            int l, r;   cin >> l >> r;
            if (dn1.query(1, 1, n, l, r) == dn2.query(1, 1, n, n - r + 1, n - l + 1)) {
                cout << "YES\n";
            }
            else {
                cout << "NO\n";
            }
        }
    }

    return 0;
}
```