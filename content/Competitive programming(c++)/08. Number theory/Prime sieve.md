**Prime Sieve** là thuật toán sàng nguyên tố, dùng để liệt kê các số nguyên tố nhỏ hơn một số n cho trước.. Điển hình nhất là Sieve of Eratosthenes.

## **Độ phức tạp sàng Eratosthenes cơ bản**

- **Thời gian**:
    - Chuẩn bị sàng trong đoạn [1, n] là O (n log log n)
    - Truy vấn trong đoạn [1, n] là O(1).
- **Không gian**: O(n), vì cần một mảng kích thước n để đánh dấu các số nguyên tố.

## **Ứng dụng**

- **Toán học và lý thuyết số**: Sử dụng để tìm các số nguyên tố trong một phạm vi nhất định. Từ đó áp dụng cho các bài toán phân tích số nguyên và bài toán lý thuyết số khác.

[Đọc thêm về sàng số nguyên tố](https://wiki.vnoi.info/algo/algebra/prime_sieve.md)
## **Kiểm tra số nguyên tố**
- Về cơ bản thì với hàm này ta có thể kiểm tra số nguyên tố 10<sup>6</sup>
```cpp
bool primeCheck(int n)
{
    if (n < 2)
        return false;
    for (int i = 2; i < n; ++i)
        if (n % i == 0)
            return false;
    return true;
}
```

## **Fermat primality test**
- Ta có thể cải tiến thuật toán trên bằng cách dùng **Phép thử Fermat(Định lý Fermat nhỏ)**
- Có thể kiểm tra được các số nguyên tố lên tới 10<sup>9</sup> tuy nhiên sẽ không đảm bảo tính chính xác 100% 
```cpp
bool probablyPrimeFermat(int n, int iter=5) {
    if (n < 4)
        return n == 2 || n == 3;

    for (int i = 0; i < iter; i++) {
        int a = 2 + rand() % (n - 3);
        if (binpower(a, n - 1, n) != 1)
            return false;
    }
    return true;
}
```

## **Miller-Rabin primality test**
- Tối ưu nhất và có thể kiểm tra lên tới 10<sup>18</sup> là dùng phép thử Miller-Rabin
```cpp
using u64 = uint64_t;
using u128 = __uint128_t;

u64 binpower(u64 base, u64 e, u64 mod) {
    u64 result = 1;
    base %= mod;
    while (e) {
        if (e & 1)
            result = (u128)result * base % mod;
        base = (u128)base * base % mod;
        e >>= 1;
    }
    return result;
}

bool check_composite(u64 n, u64 a, u64 d, int s) {
    u64 x = binpower(a, d, n);
    if (x == 1 || x == n - 1)
        return false;
    for (int r = 1; r < s; r++) {
        x = (u128)x * x % n;
        if (x == n - 1)
            return false;
    }
    return true;
};

bool MillerRabin(u64 n, int iter=5) { // returns true if n is probably prime, else returns false.
    if (n < 4)
        return n == 2 || n == 3;

    int s = 0;
    u64 d = n - 1;
    while ((d & 1) == 0) {
        d >>= 1;
        s++;
    }

    for (int i = 0; i < iter; i++) {
        int a = 2 + rand() % (n - 3);
        if (check_composite(n, a, d, s))
            return false;
    }
    return true;
}
```
### Deterministic version
```cpp
bool MillerRabin(u64 n) { // returns true if n is prime, else returns false.
    if (n < 2)
        return false;

    int r = 0;
    u64 d = n - 1;
    while ((d & 1) == 0) {
        d >>= 1;
        r++;
    }

    for (int a : {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37}) {
        if (n == a)
            return true;
        if (check_composite(n, a, d, r))
            return false;
    }
    return true;
}
```
## **Bài tập**
[Prime or not](https://www.spoj.com/problems/PON/)