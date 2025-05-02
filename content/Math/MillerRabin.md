 * Source: Wikipedia, https://miller-rabin.appspot.com/
 * Description: Deterministic Miller-Rabin primality test.
 * Guaranteed to work for numbers up to $7 \cdot 10^{18}$; for larger numbers, use Python and extend A randomly.
 * Time: 7 times the complexity of $a^b\mod c$.
```cpp
#pragma once

#include "ModMulLL.h"

bool isPrime(ull n) {
    if (n < 2 || n % 6 % 4 != 1) return (n | 1) == 3;
    
    ull witnesses[] = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37};
    
    // Tách n-1 thành d * 2^s
    ull s = __builtin_ctzll(n - 1);
    ull d = n >> s;

    // Kiểm tra với mỗi "witness"
    for (ull a : witnesses) {
        // Bỏ qua nếu a >= n
        if (a >= n) break;
        // Tính a^d % n
        ull x = modpow(a, d, n);
        if (x == 1 || x == n - 1) continue;

        for (ull r = 1; r < s; ++r) {
            x = modmul(x, x, n);
            if (x == n - 1) {
                return false;
            }
        }
    }
    return true;
}
```