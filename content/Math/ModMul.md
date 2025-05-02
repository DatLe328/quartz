 * Source: https://github.com/RamchandraApte/OmniTemplate/blob/master/src/number_theory/modulo.hpp
 * Description: Calculate $a\cdot b\bmod c$ (or $a^b \bmod c$) for $0 \le a, b \le c \le 7.2\cdot 10^{18}$.
 * Time: $O(1)$$ for $\texttt{modmul}$, $O(\log b)$$ for $\texttt{modpow}$
 * Details:
 * This runs ~2x faster than the naive (__int128_t)a * b % M.
 * A proof of correctness is in doc/modmul-proof.tex. An earlier version of the proof,
 * from when the code used a * b / (long double)M, is in doc/modmul-proof.md.
 * The proof assumes that long doubles are implemented as x87 80-bit floats; if they
 * are 64-bit, as on e.g. MSVC, the implementation is only valid for
 * $0 \le a, b \le c < 2^{52} \approx 4.5 \cdot 10^{15}$.
```cpp
#pragma once

typedef unsigned long long ull;
ull modmul(ull a, ull b, ull M) {
    ll result = a * b - M * ull(1.L / M * a * b);
    return result + M * (result < 0) - M * (result >= (ll)M);
}

ull modpow(ull base, ull exp, ull mod) {
	ull result = 1;
	while (exp > 0) {
		if (exp % 2 == 1) {
			result = modmul(result, base, mod);
		}
		base = modmul(base, base, mod);
		exp /= 2;
	}
	return result;
}
```