```cpp
#include <bits/stdc++.h>
#define ll long long
#include <fstream>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

mt19937_64 rng(chrono::steady_clock().now().time_since_epoch().count());

int rand(int l, int r) {
    return uniform_int_distribution<int>(l, r)(rng);
}
int main() {
    for (int t = 5; t <= 5; t++) {
        cout << t << '\n';
        string inp = "input/input_" + to_string(t) + ".txt";
        string out = "input/output_" + to_string(t) + ".txt";
        
        ifstream fin(inp);
        ofstream fout(out);

        if (!fin.is_open()) {
            cerr << "Error opening input file: " << inp << '\n';
            continue;
        }
        if (!fout.is_open()) {
            cerr << "Error opening output file: " << out << '\n';
            fin.close();
            continue;
        }

        int n, k;
        fin >> n >> k;
        
        vector<int> mask;
        vector<int> cnt(n + 1);
        for (int i = 0; i < (1 << n); i++) {
            int val = i ^ (i >> 1);  // Generate Gray code
            if (__builtin_popcount(val) <= k) {
                mask.push_back(val);
                cnt[__builtin_popcount(val)]++;
            }
        }
        vector<bool> used(mask.size());
        vector<int> ans = {0};
        vector<int> res = {0};
        used[0] = 1;
        cout << mask.size();
        bool stop = false;
        function<void(int, int)> solve = [&] (int val, int cur_cnt) {
            int iter = 11000;
            cout << res.size() << ' ';
            if (res.size() == 5300) {
                ans = res;
                stop = true;
                return;
            }
            if (stop) return;
            while (iter--) {
                if (stop) break;
                int i;
                int my;
                i = rand(1, mask.size() - 1);
                if (used[i]) continue;
                my = __builtin_popcount(mask[i]) + __builtin_popcount(val);
                if (my < cur_cnt) continue;
                if (__builtin_popcount(mask[i] ^ val) > 1) continue;
                cnt[__builtin_popcount(mask[i])]--;
                used[i] = 1;
                res.push_back(mask[i]);
                solve(mask[i], my);
                cnt[__builtin_popcount(mask[i])]++;
                // used[i] = 0;
                res.pop_back();
            }           
            if (res.size() > ans.size()) {
                ans = res;
            }
        };
        solve(0, 0);
        fout << ans.size() << '\n';
        for (int val : ans) {
            bitset<20> bit(val);
            string s = bit.to_string();
            fout << s.substr(20 - n) << '\n';
        }
        // Close files after each test case
        fin.close();
        fout.close();
    }
    
    return 0;
}

```