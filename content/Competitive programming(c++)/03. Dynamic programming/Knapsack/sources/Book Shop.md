```cpp
#include<bits/stdc++.h>
using namespace std;
 
int main() {
	int n, x;	cin >> n >> x;
	int w[n], val[n], dp[n + 1][x + 1];
	for (int i = 0; i < n; i++)
		cin >> w[i];
	for (int i = 0; i < n; i++)
		cin >> val[i];
	for (int i = 0; i <= x; i++)
		dp[0][i] = 0;
	for (int i = 0; i < n; i++)
		dp[i][0] = 0;
	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= x; j++) { 
			if (w[i - 1] <= j) 
				dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - w[i - 1]] + val[i - 1]);
			else
				dp[i][j] = dp[i - 1][j];
		}
	cout << dp[n][x];
	return 0;
}
```