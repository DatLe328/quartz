Start with string s = "0" then brute force build the back and front
```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int tt; cin >> tt;
    while (tt--) {
        int n; cin >> n;

        auto ask = [] (string s) -> int {
            cout << "? " << s << endl;
            int q;  cin >> q;
            return q;
        };
        string s = "0";
        bool back = true;

        if (!ask(s)) {
            cout << "! ";
            for (int i = 1; i <= n; i++) {
                cout << 1;
            }
            cout << endl;
            continue;
        }
        while (s.size() < n) {
            if (back) {
                if (ask(s + "0")) {
                    s = s + "0";
                }
                else if (ask(s + "1")) {
                    s = s + "1";
                }
                else {
                    back = false;
                }
            }
            else {
                if (ask("1" + s)) {
                    s = "1" + s;
                }
                else {
                    s = "0" + s;
                }
            }
        }
        cout << "! " << s << endl;
    }
    return 0;
}
```