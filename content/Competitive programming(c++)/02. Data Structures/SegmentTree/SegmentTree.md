```cpp
template<typename T>
class SegmentTree {
private:
    T INF = 1e9;
    int n;
    vector<T> st;

    T F(T a, T b) {
        return min(a, b);
    }
    void build(const vector<T>& arr, int id, int l, int r) {
        if (l == r) {   st[id] = arr[l];    return; }
        int mid = (l + r) / 2;
        build(arr, id * 2, l, mid);
        build(arr, id * 2 + 1, mid + 1, r);
        st[id] = F(st[id * 2], st[id * 2 + 1]);
    }
    void update(int id, int l, int r, int i, T val) {
        if (l > i || r < i) {   return; }
        if (l == r) {   st[id] = val;   return; }
        int mid = (l + r) / 2;
        update(id * 2, l, mid, i, val);
        update(id * 2 + 1, mid + 1, r, i, val);
        st[id] = F(st[id * 2], st[id * 2 + 1]);
    }
    T query(int id, int l, int r, int u, int v) {
        if (l > v || r < u) {   return INF; }
        if (l >= u && r <= v) { return st[id];  }
        int mid = (l + r) / 2;
        return F(query(id * 2, l, mid, u, v), query(id * 2 + 1, mid + 1, r, u, v));
    }
public:
    SegmentTree() {}
    SegmentTree(const int& n) : n(n), st(n * 4, INF) {}
    SegmentTree(const vector<T>& arr, const int& n) : n(n), st(n * 4, INF) {
        build(arr, 1, 1, n);
    }
    void update(int i, T val) {
        update(1, 1, n, i, val);
    }
    T query(int l, int r) {
        return query(1, 1, n, l, r);
    }
};

```

```cpp
template<typename T>
class SegmentTreeLazy {
private:
    T INF = 1e9;
    int n;
    vector<T> st, lazy;

    T F(T a, T b) {
        return max(a, b);
    }
    
    void push(int id) {
        if (lazy[id]) {
            st[id * 2] += lazy[id];
            st[id * 2 + 1] += lazy[id];
            lazy[id * 2] += lazy[id];
            lazy[id * 2 + 1] += lazy[id];
            lazy[id] = 0;
        }
    }
    void build(const vector<T>& arr, int id, int l, int r) {
        if (l == r) {   st[id] = arr[l];    return; }
        int mid = (l + r) / 2;
        build(arr, id * 2, l, mid);
        build(arr, id * 2 + 1, mid + 1, r);
        st[id] = F(st[id * 2], st[id * 2 + 1]);
    }

    void update(int id, int l, int r, int i, int val) {
        if (l > i || r < i) {   return; }
        if (l == r) {   st[id] = val;   return; }
        int mid = (l + r) / 2;
        update(id * 2, l, mid, i, val);
        update(id * 2 + 1, mid + 1, r, i, val);
        st[id] = F(st[id * 2], st[id * 2 + 1]);
    }

    void rangeUpdate(int id, int l, int r, int u, int v, T val) {
        if (l > v || r < u) {   return; }
        if (l >= u && r <= v) {   
            st[id] += val;   
            lazy[id] += val;
            return; 
        }
        push(id);
        int mid = (l + r) / 2;
        rangeUpdate(id * 2, l, mid, u, v, val);
        rangeUpdate(id * 2 + 1, mid + 1, r, u, v, val);
        st[id] = F(st[id * 2], st[id * 2 + 1]);
    }
    T query(int id, int l, int r, int u, int v) {
        if (l > v || r < u) {   return INT_MIN; }
        if (l >= u && r <= v) { return st[id];  }
        push(id);
        int mid = (l + r) / 2;
        return F(query(id * 2, l, mid, u, v), query(id * 2 + 1, mid + 1, r, u, v));
    }
public:
    SegmentTreeLazy() {}
    SegmentTreeLazy(const int& n) : n(n), st(n * 4, 0), lazy(n * 4, 0) {}
    SegmentTreeLazy(const vector<T>& arr, const int& n) : n(n), st(n * 4, 0), lazy(n * 4, 0) {
        build(arr, 1, 1, n);
    }
    void update(int i, int val) {
        update(1, 1, n, i, val);
    }
    void rangeUpdate(int l, int r, T val) {
        rangeUpdate(1, 1, n, l, r, val);
    }
    T query(int l, int r) {
        return query(1, 1, n, l, r);
    }
};

```