If we need all all the totient of all numbers between   1<math xmlns="http://www.w3.org/1998/Math/MathML"><mn>1</mn></math>$1$  and   n<math xmlns="http://www.w3.org/1998/Math/MathML"><mi>n</mi></math>$n$ , then factorizing all   n<math xmlns="http://www.w3.org/1998/Math/MathML"><mi>n</mi></math>$n$  numbers is not efficient. We can use the same idea as the [Sieve of Eratosthenes](https://cp-algorithms.com/algebra/sieve-of-eratosthenes.html). It is still based on the property shown above, but instead of updating the temporary result for each prime factor for each number, we find all prime numbers and for each one update the temporary results of all numbers that are divisible by that prime number.

Since this approach is basically identical to the Sieve of Eratosthenes, the complexity will also be the same:   O(nlog⁡log⁡n)<math xmlns="http://www.w3.org/1998/Math/MathML"><mi>O</mi><mo stretchy="false">(</mo><mi>n</mi><mi>log</mi><mo data-mjx-texclass="NONE">⁡</mo><mi>log</mi><mo data-mjx-texclass="NONE">⁡</mo><mi>n</mi><mo stretchy="false">)</mo></math>$O(n \log \log n)$
```cpp
void phi_1_to_n(int n) {
    vector<int> phi(n + 1);
    for (int i = 0; i <= n; i++)
        phi[i] = i;

    for (int i = 2; i <= n; i++) {
        if (phi[i] == i) {
            for (int j = i; j <= n; j += i)
                phi[j] -= phi[j] / i;
        }
    }
}
```