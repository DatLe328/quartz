# Hashing (băm)

**Hashing** là kỹ thuật chuyển đổi dữ liệu thành các giá trị băm (hash values) để truy xuất dữ liệu một cách nhanh chóng.

## Độ phức tạp

Đối với bài toán so khớp chuỗi độ dài m trong một chuỗi độ dài n, độ phức tạp là:

- **Thời gian:** O(n + m)
- **Không gian:** O(n)

## Ứng dụng

- Bảng băm (hash table) cho các thao tác tra cứu, chèn, và xóa nhanh.
- Kiểm tra chuỗi con (substring), so khớp mẫu.
- Có thể kết hợp với tìm kiếm nhị phân để tìm xâu palindrome dài nhất

```cpp
const int MAX_N = 1e6 + 1;
const ll MOD = 1e9 + 7;
const int base = 127;

ll POW[MAX_N];
void init_pow() {
    POW[0] = 1;
    for (int i = 1; i < MAX_N; i++) {
        POW[i] = (POW[i - 1] * base) % MOD;
    }
}
struct Hash {
    vector<ll> h, rh;
    Hash(string s)  {
        int n = (int)s.size();
        h.resize(n + 1, 0);
        rh.resize(n + 2, 0);
        for (int i = 1; i <= n; i++) {
            h[i] = (h[i - 1] * base + s[i - 1] - 'a' + 1) % MOD;
        }
        for (int i = n; i > 0; i--) {
            rh[i] = (rh[i + 1] * base + s[i - 1] - 'a' + 1) % MOD;
        }
    }

    ll get_hash(int l, int r) {
        return (h[r] - h[l - 1] * POW[r - l + 1] + MOD * MOD) % MOD;
    }
    ll get_rhash(int l, int r) {
        return (rh[l] - rh[r + 1] * POW[r - l + 1] + MOD * MOD) % MOD;
    }
    bool is_palindrome(int l, int r) {
        return get_hash(l, r) == get_rhash(l, r);
    }
    bool check(int l, int n) {
        for (int i = 1; i <= n - l + 1; i++) {
            int j = i + l - 1;
            if (is_palindrome(i, j)) {
                return true;
            }
        }
        return false;
    }
};
```
https://oj.vnoi.info/problem/dtksub
https://oj.vnoi.info/problem/substr/