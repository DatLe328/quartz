```cpp
#include<bits/stdc++.h>
using namespace std;

template<typename T>
struct FenwickTree {
    int n;
    vector<T> bit;
    FenwickTree(int n) : n(n), bit(n + 1, 0) {}

    inline int LSOne(int s) {
        return s & (-s);
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

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n, L, R;
    cin >> n >> L >> R;
    vector<int> a(n);
    for (auto &i : a) {
        cin >> i;
    }

    // Tính prefix sum
    vector<long long> pref(n + 1, 0);
    for (int i = 1; i <= n; ++i) {
        pref[i] = pref[i - 1] + a[i - 1];
    }

    // Nén các giá trị của prefix sum để sử dụng trong Fenwick Tree
    vector<long long> d = pref;
    sort(d.begin(), d.end());
    d.erase(unique(d.begin(), d.end()), d.end());

    for (int i = 0; i <= n; ++i) {
        pref[i] = lower_bound(d.begin(), d.end(), pref[i]) - d.begin() + 1;
    }

    // Khởi tạo Fenwick Tree với kích thước đã nén
    FenwickTree<int> bit(d.size());
    long long count = 0;

    // Duyệt qua các prefix sum và tính số cặp phù hợp
    for (int j = 0; j <= n; ++j) {
        // Tìm chỉ số của S[j] - R và S[j] - L trong d để đếm số lượng phù hợp
        int left_bound = lower_bound(d.begin(), d.end(), d[pref[j] - 1] - R) - d.begin() + 1;
        int right_bound = upper_bound(d.begin(), d.end(), d[pref[j] - 1] - L) - d.begin();

        // Đếm số lượng prefix sum trong khoảng từ left_bound đến right_bound
        if (left_bound <= right_bound) {
            count += bit.range_query(left_bound, right_bound);
        }

        // Thêm prefix[j] vào Fenwick Tree
        bit.update(pref[j], 1);
    }

    cout << count << '\n';
    return 0;
}

```