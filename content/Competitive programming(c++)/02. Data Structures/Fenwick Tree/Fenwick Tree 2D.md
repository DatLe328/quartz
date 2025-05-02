```cpp
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
```