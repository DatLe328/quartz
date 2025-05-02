## A. With Exponentiation
[**Fermat's Little Theorem**](https://en.wikipedia.org/wiki/Fermat%27s_little_theorem)**Last**$ap−1≡1(modp)a^{p - 1} \equiv 1$ \pmod{p}ap−1≡1(modp)ap−2⋅a≡1(modp)a^{p-2} \cdot a \equiv 1 \pmod{p}ap−2⋅a≡1(modp)ap−2a^{p - 2}ap−2aaappp
 * Description: Pre-computation of modular inverses. Assumes $LIM \le mod$ and that mod is a prime.
```cpp
#pragma once

// const ll mod = 1000000007, LIM = 200000; ///include-line
ll* inv = new ll[LIM] - 1; inv[1] = 1;
rep(i,2,LIM) inv[i] = mod - (mod / i) * inv[mod % i] % mod;
```