**Ternary search** is a divide-and-conquer algorithm used to find the maximum or minimum of a unimodal function, or to search for an element in a sorted array. It works by dividing the search space into three parts, rather than two as in binary search. Specifically, two midpoints are chosen, dividing the array into three segments. The algorithm then compares values at these midpoints and determines which segment contains the target (or optimal point), discarding the other two segments. This process repeats until the search space is reduced to the desired precision or target.

Ternary search is especially useful when working with unimodal functions, which have a single peak (or trough). It has a time complexity of $O(\log_3 n)$, making it efficient, but typically less common than binary search due to the specificity of its use cases.
```cpp
for (int i = 0; i < N_ITER; i++) {

	double x1 = left + (right - left) / 3.0;
	double x2 = right - (right - left) / 3.0;

	if (f(x1) < f(x2)) right = x2;
	else left = x1;
}
```

```cpp
ll left = 1;
ll right = 1e12;
while (right - left > 4) {
	ll x1 = left + (right - left) / 3.0;
	ll x2 = right - (right - left) / 3.0;
	if (f(x1) > f(x2)) left = x1;
	else right = x2;
}
ll ans = 1e18;
for (int i = left; i <= right; i++) {
	ans = min(ans, f(i));
}
```