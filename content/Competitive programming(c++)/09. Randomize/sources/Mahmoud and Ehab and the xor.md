```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

ll start = chrono::steady_clock::now().time_since_epoch().count();
mt19937_64 rng(start);

int n, x;   
vector<int> a;

/**
 * Random number from [0, n - 1)
 * Then we can use xor to calculate the last number
 * **/
bool solve() {
    shuffle(a.begin(), a.end(), rng);
    int res = 0;
    for (int i = 0; i < n - 1; i++) {
        res ^= a[i];
    }
    int last_number = x ^ res;
    // Check if the last number is distinct
    for(int i = 0 ; i < n - 1 ; ++ i)
		if( last_number == a[i] )
			return false;
    // The last number must in range [0, 1e6]
    if (last_number > 1e6) {
        return false;
    }
	return true;
}
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

    cin >> n >> x;
    for (int i = 0; i < 1000000; i++) {
        a.push_back(i);
    }
    if (n == 2 && x == 0) {
        cout << "no\n";
    }
    else {
        cout << "YES\n";
        while (!solve());
        for (int i = 0; i < n - 1; i++) {
            cout << a[i] << ' ';
            x ^= a[i];
        }
        cout << x;
    }
	return 0;
}
```