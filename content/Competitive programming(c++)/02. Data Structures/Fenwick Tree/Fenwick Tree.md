```cpp
template<typename T>
struct FenwickTree {
    int n;
    vector<T> bit;
    FenwickTree(int n) : n(n), bit(n + 1, 0) {};

    inline int LSOne(int s) {
        return (s) & (-s);
    }
    void update(int i, int v) {
        while (i <= n) {
            bit[i] += v;
            i += LSOne(i);
        }
    }

    T query(int i) {
        T ret = 0;
        while (i > 0) {
            ret += bit[i];
            i -= LSOne(i);
        }
        return ret;
    }

    T range_query(int l, int r) {
        return query(r) - query(l - 1);
    }
};
```