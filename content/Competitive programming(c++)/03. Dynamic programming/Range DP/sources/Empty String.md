```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const ll MOD = 1e9 + 7;
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

	string s;
	cin >> s;
    int n = (int)s.size();
    vector<vector<ll>> dp(n + 1, vector<ll>(n + 1));
    vector<vector<ll>> choose(n / 2 + 1, vector<ll>(n / 2 + 1));
	n = (int)s.size();

	choose[0][0] = 1;
	for (int i = 1; i <= n / 2; ++i) {
		choose[i][0] = 1;
		for (int j = 1; j <= i; ++j)
			choose[i][j] = (choose[i - 1][j] + choose[i - 1][j - 1]) % MOD;
	}

	for (int i = 0; i + 1 <= n; ++i) dp[i + 1][i] = 1;

	for (int i = n - 1; i >= 0; --i) {
		for (int j = i + 1; j < n; j += 2) {
			for (int k = i + 1; k <= j; k += 2) {
				if (s[i] == s[k]) {
					int temp = (dp[i + 1][k - 1] * dp[k + 1][j]) % MOD;
					temp = (temp * choose[(j - i + 1) / 2][(k - i + 1) / 2]) % MOD;
					dp[i][j] = (dp[i][j] + temp) % MOD;
				}
            }
        }
    }
	cout << dp[0][n - 1] << '\n';

    return 0;
}
```