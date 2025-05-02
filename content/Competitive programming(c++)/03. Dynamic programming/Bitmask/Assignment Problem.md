> **Problem 6.1.1 (Assignment Problem)**
> There are $n$ people numbered $1...n$ who are looking for rides and need to be assigned to $n$ taxis also numbered $1...n$. There is also a cost matrix, where $cost[i][j]$ denotes the cost of letting person $i$ ride taxi $j$. Each person needs to be assigned a taxi, and each taxi must be assigned to exactly one person. What is the minimum total cost.

The naive solution is to try all $n!$ assignments and pick the best one. This obviously takes $O(n!)$ time, which is not good. How can we do better? We can notice that for some subset of size $k$ of taxis that are filled with the first $k$ people, we don’t care about how it’s filled, but rather the minimum cost of it being filled. This makes sense when considering what we’re looking for: the minimum cost of filling the size $n$ subset. More specifically, we don’t care about how the people are permuted in a taxi subset consisting of the first $k$ people. This already hints at reducing the time complexity from factorial to exponential.

First, you should be familiar with how to represent subsets with bitmasks. This is a common technique when generating subsets. Notice that the only set bit in $2k$ is in the kth position from the right. All subsets of the bits from position $0$ to $k −1$ are generated when enumerating from $0$ to $2k −1$. Consider the sets of positions (from the right) of set bits for $k = 3$:
$$\begin{aligned} 000 &= \{\} \\ 001 &= \{0\} \\ 010 &= \{1\} \\ 011 &= \{0, 1\} \\ 100 &= \{2\} \\ 101 &= \{0, 2\} \\ 110 &= \{1, 2\} \\ 111 &= \{0, 1, 2\} \end{aligned}$$
![[Pasted image 20241006110201.png]]
This is because the arrows are all pointed in the same direction, enabling optimal substructure. Proving this is always the case is left as an exercise for the reader.
```cpp
for (int mask = 0; mask < (1 << n); mask++) {
	for (int i = 0; i < n; i++) {
		if (mask & (1 << i) == 0) continue;
		dp[mask | (1 << i)] = min(dp[mask | (1 << i)], cost[mask][i]);
	}
}
cout << dp[(1 << n) - 1];
```
