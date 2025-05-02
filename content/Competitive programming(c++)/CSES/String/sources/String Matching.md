# Solution 1: KMP
```cpp
#include <bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

vector<int> prefix_function(string s) {
    int n = (int)s.length();
    vector<int> pi(n);
    for (int i = 1; i < n; i++) {
        int j = pi[i-1];
        while (j > 0 && s[i] != s[j])
            j = pi[j-1];
        if (s[i] == s[j])
            j++;
        pi[i] = j;
    }
    return pi;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    string a, b;    cin >> a >> b;
    string s = b + "#" + a;
    int n = a.size();
    int m = b.size();
    auto pi = prefix_function(s);
    // debug(pi);
	int j = 0;
    int ans = 0;
	for (int i = 0; i < n; i++) {
		while (j > 0 && a[i] != b[j]) {
			j = pi[j - 1];
		}
		if (a[i] == b[j]) {
			j++;
		}
		if (j == m) {
            ans++;
		}
	}
    cout << ans;
    return 0;
}

```
# #Solution 2: Hash
```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
const int base = 127;
const ll mod = 1e9 + 7;
const ll maxn = 1e6 + 1;
 
ll POW[maxn], hashP[maxn];
 
ll getHashP(int i, int j) {
	return (hashP[j] - hashP[i - 1] * POW[j - i + 1] + mod * mod) % mod;
}
int main() {
	string P, T;	cin >> P >> T;
	int n = P.size();
	int m = T.size();
	P = " " + P;
	T = " " + T;
	POW[0] = 1;
	for (int i = 1; i <= n; i++)
		POW[i] = (POW[i - 1] * base) % mod;
 
	ll hashT = 0;
	for (int i = 1; i <= n; i++)
		hashP[i] = (hashP[i - 1] * base + P[i] - 'a' + 1) % mod;
	for (int i = 1; i <= m; i++)
		hashT = (hashT * base + T[i] - 'a' + 1) % mod;
	int cnt = 0;
	for (int i = 1; i <= n; i++) {
		if (getHashP(i, i + m - 1) == hashT)
			cnt++;
	}
	cout << cnt;
	return 0;
}
```
# Solution 3: Suffix Array
```cpp

```