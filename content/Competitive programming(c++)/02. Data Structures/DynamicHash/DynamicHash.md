```cpp
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
```