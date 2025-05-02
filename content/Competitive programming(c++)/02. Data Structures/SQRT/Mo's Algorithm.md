**Mo's Algorithm** là một thuật toán xử lý truy vấn trên mảng bằng cách sắp xếp và xử lý các truy vấn theo các khối chia căn bậc hai của kích thước mảng, giúp tối ưu hóa độ phức tạp.
```cpp
struct Query {
    int id, l, r;
    Query() {}
    Query(int id, int l, int r) : id(id), l(l), r(r) {}
 
    bool operator< (Query b) {
        Query a = *this;
        return make_pair(a.l / d, a.r) < make_pair(b.l / d, b.r);
    }
} qs[MAX_N];

void add(int idx) {
	// Implement add function
}
void remove(int idx) {
	// Implement remove function
}
int get_ans() {
	// Implement get_ans function
}
void MoAlgorithm() {
    sort(qs + 1, qs + q + 1);
    int curL = 1, curR = 1;
    add(1);
    for (int i = 1; i <= q; i++) {
        int l = qs[i].l, r = qs[i].r;
        while (curL > l) { add(--curL); }
        while (curL < l) { remove(curL++); }
        while (curR > r) { remove(curR--); }
        while (curR < r) { add(++curR); }
        ans[qs[i].id] = get_ans();
    }
}
```