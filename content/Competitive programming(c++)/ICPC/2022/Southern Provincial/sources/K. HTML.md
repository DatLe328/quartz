```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

vector<tuple<string, int, int, int>> a = {
{ "White", 255, 255, 255 },
{ "Silver", 192, 192, 192 },
{ "Gray", 128, 128, 128 },
{ "Black", 0, 0, 0 },
{ "Red", 255, 0, 0 },
{ "Maroon", 128, 0, 0 },
{ "Yellow", 255, 255, 0 },
{ "Olive", 128, 128, 0 },
{ "Lime", 0, 255, 0 },
{ "Green", 0, 128, 0 },
{ "Aqua", 0, 255, 255 },
{ "Teal", 0, 128, 128 },
{ "Blue", 0, 0, 255 },
{ "Navy", 0, 0, 128 },
{ "Fuchsia", 255, 0, 255 },
{ "Purple", 128, 0, 128 }};

void read_input() {
    string s; 
    while (getline(cin, s)) {
        stringstream ss(s);
        string word;
        vector<string> tmp;
        while (ss >> word) {
            tmp.push_back(word);
        }
        cout << "{ ";
        cout << "\"" + tmp[1] + "\", ";
        for (int i = 2; i < 4; i++) {
            cout << tmp[i] << ", ";
        }
        cout << tmp[4] << " },\n";
    }
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int r, g, b; 
    while (true) {
        cin >> r >> g >> b;
        if (r == -1) break;
        int best = 0;
        double d = 1e9;
        for (int i = 0; i < 16; i++) {
            auto [s, u, v, t] = a[i];
            double tmp = sqrt(pow(u - r, 2) + pow(v - g, 2) + pow(t - b, 2));
            if (tmp < d) {
                best = i;
                d = tmp;
            }
        }
        cout << get<0>(a[best]) << '\n';
    }
    return 0;
}
```