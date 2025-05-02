```cpp
#include<bits/stdc++.h>
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

	string s;	cin >> s;
	auto p = prefix_function(s);
	int x = p[s.size() - 1];
	if (x <= s.size() / 2) {
		cout << "NO\n";
	}
	else {
		cout << "YES\n";
		cout << s.substr(0, x);
	}

	return 0;
}
```