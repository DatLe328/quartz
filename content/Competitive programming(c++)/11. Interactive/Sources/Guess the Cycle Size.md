When we ask the distance between a and b it will give 1 of 2 path with probility 1/2, simple we just need to ask 1 to i and i to 1, this will give us high probility of correct answer, we loop from 2 to 25 because the problem says that we can ask up to 50 queries, if it return -1 it mean there are the cycle size is i-1, if it return different value that means it return 2 paths so we can easyly answer
```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

ll ask(ll a, ll b) {
	cout << "? " << a << " " << b << endl;
	ll ret;	cin >> ret;
	return ret;
}
int main() {
	for (int i = 2; i < 26; i++) {
		ll a = ask(1, i);
		ll b = ask(i, 1);
		if (a == -1) {
			cout << "! " << i - 1 << endl;
			break;
		}
		else if (a != b) {
			cout << "! " << a + b << endl;
			break;
		}
	}

	return 0;
}
```