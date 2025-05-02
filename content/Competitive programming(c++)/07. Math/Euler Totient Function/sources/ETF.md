```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

vector<int> phi_1_to_n(int n) {
    vector<int> phi(n + 1);
    for (int i = 0; i <= n; i++)
        phi[i] = i;

    for (int i = 2; i <= n; i++) {
        if (phi[i] == i) {
            for (int j = i; j <= n; j += i)
                phi[j] -= phi[j] / i;
        }
    }
    return phi;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    vector<int> phi = phi_1_to_n(1000000);
    int tt; cin >> tt;
    while (tt--) {
        int n;  cin >> n;
        cout << phi[n] << '\n';
    }

    return 0;
}
```