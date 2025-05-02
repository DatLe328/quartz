```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

template<typename T>
struct FenwickTree {
    int n, m;
    vector<vector<T>> bit;
    FenwickTree(int n, int m) : n(n), m(m), bit(n + 1, vector<T>(m + 1, 0)) {}

    void update(int r, int c, T val) {
        while (r <= n) {
            for (int k = c; k <= m; k += k & (-k)) {
                bit[r][k] += val;
            }
            r += r & (-r);
        }
    }
    T sum(int r, int c) {
        T ret = 0;
        while (r) {
            for (int k = c; k > 0; k -= k & (-k)) {
                ret += bit[r][k];
            }
            r -= r & (-r);
        }
        return ret;
    }
    T rect_sum(int r1, int c1, int r2, int c2) {
        return sum(r2, c2) - sum(r2, c1 - 1) - sum(r1 - 1, c2) +
		       sum(r1 - 1, c1 - 1);
    }

};
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n, q;   cin >> n >> q;
    FenwickTree<int64_t> fen(n, n);
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            char c; cin >> c;
            if (c == '*') fen.update(i, j, 1);
        }
    }
    while (q--) {
        int t;  cin >> t;
        if (t == 1) {
            int r, c;   cin >> r >> c;
            if (fen.rect_sum(r, c, r, c) == 1) {
                fen.update(r, c, -1);
            }
            else {
                fen.update(r, c, 1);
            }
        }
        else {
            int r1, c1, r2, c2; cin >> r1 >> c1 >> r2 >> c2;
            cout << fen.rect_sum(r1, c1, r2, c2) << '\n';
        }

    }

    return 0;
}
```