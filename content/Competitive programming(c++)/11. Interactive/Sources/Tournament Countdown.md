There is a way to erase 3 participants in every 2 queries. Since there are $2^n - 1$ participants to be removed, the number of queries will be 

$$\left\lceil( 2^{n} - 1) \cdot \frac{2}{3}\right\rceil = \left\lfloor \frac{2^{n+1}}{3} \right\rfloor$$

Suppose there are only 4 participants. In the first query, we will ask the judge to compare the **1st** and the **3rd** participants. There are three cases:

- The **1st** participant wins more games than the **3rd** one: the **2nd** and **3rd** cannot be the winner.
- The **3rd** participant wins more games than the **1st** one: the 1st and **4th** cannot be the winner.
- The **1st** and **3rd** participants' numbers of winning games are equal: both the **1st** and **3rd** cannot be the winner.

Ask the remaining two participants, find the winner between them.

If there are more than 4 participants, we can continuously divide the number by 4 again and again, until there are at most 2 participants left. Now we can get the winner in one final query.

```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

int ask(int a, int b) {
	cout << "? " << a << ' ' << b << endl;
	int ret;	cin >> ret;
	return ret;
}
int main() {
	int tt;	cin >> tt;
	while (tt--) {
		int n;	cin >> n;
		vector<int> a(1 << n);
		iota(a.begin(), a.end(), 1);
		while (a.size() >= 4) {
			vector<int> b;
			for (int i = 0; i < a.size(); i += 4) {
				int q = ask(a[i], a[i + 2]);
				if (q == 0) {
					if (ask(a[i + 1], a[i + 3]) == 1) {
						b.push_back(a[i + 1]);
					}
					else {
						b.push_back(a[i + 3]);
					}
				}
				else if (q == 1) {
					if (ask(a[i], a[i + 3]) == 1) {
						b.push_back(a[i]);
					}
					else {
						b.push_back(a[i + 3]);
					}
				}
				else {
					if (ask(a[i + 1], a[i + 2]) == 1) {
						b.push_back(a[i + 1]);
					}
					else {
						b.push_back(a[i + 2]);
					}
				}
			}
			a = b;
		}
		if (ask(a[0], a[1]) == 1) {
			cout << "! " << a[0] << endl;
		}
		else {
			cout << "! " << a[1] << endl;
		}
	}

	return 0;
}
```