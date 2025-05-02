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

    int tt; cin >> tt;
    while (tt--) {
        string str;   cin >> str;
        int n = int(str.size());
        vector<int> rock(n), paper(n), scissor(n);
        rock[0] = str[0] == 'R';
        paper[0] = str[0] == 'P';
        scissor[0] = str[0] == 'S';
        for (int i = 1; i < n; i++) {
            if (str[i] == 'R') rock[i]++;
            if (str[i] == 'P') paper[i]++;
            if (str[i] == 'S') scissor[i]++;
            rock[i] += rock[i - 1];
            paper[i] += paper[i - 1];
            scissor[i] += scissor[i - 1];
        }
        
        auto get_max = [&] (int l, int r) -> int {
            if (l == 0) {
                return max({rock[r], paper[r], scissor[r]});
            }
            int res1 = rock[r] - rock[l - 1];
            int res2 = paper[r] - paper[l - 1];
            int res3 = scissor[r] - scissor[l - 1];
            return max({res1, res2, res3});
        };
        auto get_val = [&] (int len) -> int {
            int ret = 0;
            for (int left = 0; left < n; left += len) {
                int right = min(n - 1, left + len - 1);
                ret += get_max(left, right);
            }
            cout << len << ' ' << ret << '\n';
            return ret;
        };
        int l = 1, r = n;
        while (r - l > 2) {
            int left = l + (r - l) / 3;
            int right = r - (r - l) / 3;
            if (get_val(right) >= get_val(left)) {
                l = left;
            }
            else {
                r = right;
            }
        }
        cout << l << ' ' << r << '\n';
        cout << "================\n";
    }

    return 0;
}
```