```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    string s;
    string t;   
    while (getline(cin, t)) {
        s += " " + t;
    }
    stringstream ss(s);
    string word;
    set<string> res;
    while (ss >> word) {
        res.insert(word);
    }
    string tmp[] {"Welcome", "Hue" ,"University", "of", "Sciences"};
    set<string> solve;
    for (string i : tmp) {
        solve.insert(i);
    }
    for (string i : res) {
        if (solve.find(i) != solve.end()) {
            solve.erase(solve.find(i));
        }
    }
    if (solve.size()) {
        cout << "No";
    }
    else {
        cout << "Yes";
    }

    return 0;
}
```