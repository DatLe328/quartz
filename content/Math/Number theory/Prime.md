# Miller Rabin
```mathematica
Function isPrime(n):
    If n < 2 OR n % 6 % 4 ≠ 1:
        Return (n OR 1) == 3

    List of witnesses = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37]

    // Decompose n - 1 into d * 2^s
    s = number of trailing zero bits in (n - 1)
    d = (n - 1) / 2^s

    // Check against each witness
    For each witness a in the list:
        If a ≥ n:
            Break the loop

        x = modpow(a, d, n)  // Compute a^d % n
        If x == 1 OR x == n - 1:
            Continue to the next witness

        For r from 1 to s - 1:
            x = modmul(x, x, n)  // Square x modulo n
            If x == n - 1:
                Return false  // n is not prime

    Return true  // n is prime

```
# Number theoretic functions
With $n=p_1^{a_1}p_2^{a_2}...p_k^{a_k}$
## Number of factors
https://www.spoj.com/problems/COMDIV/
$$\tau(n)=\prod_{i=1}^{k}{(a_i+1)}$$
## Sum of factors
$$\sigma(n)=\prod_{i=1}^{k}{(1+p_i+...+p_i^{a_i})}=\prod_{i=1}^{k}{\frac{p_{i}^{a_{i+1}}- 1}{p_i-1}}$$
## Product of factors
$$\mu(n)=n^{\tau(n)/2}$$
# Conjectures
There are many *conjectures* involving primes. Most people think that the conjectures are true, but nobody has been able to prove them. For example, the following conjectures are famous:

• **Goldbach’s conjecture**: Each even integer $n > 2$ can be represented as a
sum $n=a+b$ so that both $a$ and $b$ are primes.
• **Twin prime conjecture**: There is an infinite number of pairs of the form
$\{p,\ p^2\}$, where both $p$ and $p+2$ are primes.
• **Legendre’s conjecture**: There is always a prime between numbers $n^2$
and $(n+1)^2$, where $n$ is any positive integer.
# Euler’s totient function
[Read more](https://cp-algorithms.com/algebra/phi-function.html)

Numbers $a$ and $b$ are **coprime** if $\gcd(a, b) = 1$. 
**Euler’s totient function** $\varphi(n)$ gives the number of coprime numbers to $n$ between $1$ and $n$. 
For example, $\varphi(12) = 4$, because $1, 5, 7$ and $11$ are coprime to $12$.

The value of $\varphi(n)$ can be calculated from the prime factorization of $n$ using the formula:
$$\varphi(n) = \prod_{i=1}^{k} p_i^{a_i - 1} (p_i - 1).$$

For example,
$$\varphi(12) = 2^1 \cdot (2 - 1) \cdot 3^0 \cdot (3 - 1) = 4.$$
Note that $\varphi(n) = n - 1$ if $n$ is prime.