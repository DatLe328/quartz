**Description:** You have a set of objects, and want to divide them into pairs such that the total pairing cost is minimum or maximum.
**Bitmask:** Bitmask represents paired objects.
**DP Status:**
$dp[mask]$: is the minimum/maximum cost when combining the objects in the mask into pairs.

Step 1: Calculate cost between pair $[i, j]$ and assign into $mask$ that contain pair$[i, j]$ 
Step 2: Loop all reverse_mask's submask and use $OR$ to create new mask that contain new bit have been inserted into $mask$ and get min/max cost in the new mask
```cpp
dp[0][0] = 1;
for (int mask = 1; mask < (1 << n); mask++) {
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (mask & (1 << i) && mask & (1 << j)) {
                    cost[mask] += c[i][j]; // Cost between i and j
                }
            }
        }
    }
}
for (int mask = 0; mask < (1 << n); mask++) {
	int reverse_mask = ((1 << n) - 1) ^ mask;
	// loop all reverse_mask's submask
	while (int k = reverse_mask; k > 0; k = (k - 1) & reverse_mask) {
		dp[mask | k] = max(dp[mask | k], dp[mask] + cost[k]);
	}
}
```