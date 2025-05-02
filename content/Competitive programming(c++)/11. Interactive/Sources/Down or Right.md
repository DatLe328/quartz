```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

bool query(int i, int j, int y, int x) {
    cout << "? " << i << ' ' << j << ' ' << y << ' ' << x << endl;
    string s;   cin >> s;
    return s == "YES";
}
int main() {
    int n;  cin >> n;
    int i = 1, j = 1;
    string l = "", r = "";
    while (i + j < n + 1) {
        if (i + 1 <= n && query(i + 1, j, n, n)) {
            i++;
            l += "D";
        }
        else {
            j++;
            l += "R";
        }
    }
    i = n, j = n;
    while (i + j > n + 1) {
        if (i - 1 > 0 && query(1, 1, i, j - 1)) {
            j--;
            r += "R";
        }
        else {
            i--;
            r += "D";
        }
    }
    reverse(r.begin(), r.end());
    cout << "! " << l + r << endl;

    return 0;
}
```