```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

void count_sort(vector<int>& p, const vector<int> c) {
    int n = (int)p.size();
    vector<int> cnt(n), pos(n), p_new(n);
    for (auto i : c) {
        cnt[i]++;
    }
    for (int i = 1; i < n; i++) {
        pos[i] = pos[i - 1] + cnt[i - 1];
    }
    for (auto i : p) {
        p_new[pos[c[i]]] = i;
        pos[c[i]]++;
    }
    swap(p, p_new);
}
pair<vector<int>, vector<int>> compute_suffix(string s) {
    s += 32;
    int n = (int)s.size();
    vector<int> p(n), c(n), c_new(n);

    iota(p.begin(), p.end(), 0);
    sort(p.begin(), p.end(), [&] (int l, int r) { return s[l] < s[r]; });
    for (int i = 1; i < n; i++) {
        c[p[i]] = c[p[i - 1]] + int(s[p[i]] != s[p[i - 1]]);
    }
    for (int k = 0; (1 << k) <= n; k++) {
        int len = (1 << k);
        for (int i = 0; i < n; i++) {
            p[i] = (p[i] - len + n) % n;
        }
        count_sort(p, c);
        for (int i = 1; i < n; i++) {
            pair<int, int> prev = {c[p[i - 1]], c[(p[i - 1] + len) % n]};
            pair<int, int> cur = {c[p[i]], c[(p[i] + len) % n]};
            c_new[p[i]] = c_new[p[i - 1]] + int(prev != cur);
        }
        swap(c, c_new);
    }
    return {p, c};
}
vector<int> compute_lcp(const vector<int>& p, const vector<int>& c, const string& s) {
    int n = (int)s.size();
    vector<int> lcp(n);
    int k = 0;
    for (int i = 0; i < n; i++) {
        int pi = c[i];
        int j = p[pi - 1];
        while (s[i + k] == s[j + k]) {
            k++;
        }
        lcp[pi - 1] = k;
        k = max(k - 1, 0);
    }
    return lcp;
}

string k_th_substring(vector<int> p, vector<int> lcp, string s, int k) {
    int n = (int)s.size();
    for (int i = 1; i <= n; i++) {  // Suffix bắt đầu từ 1
        int unique_substrings = (n - p[i]) - lcp[i - 1];
        if (k <= unique_substrings) {
            return s.substr(p[i], k + lcp[i - 1]);
        }
        k -= unique_substrings;
    }
    return "";
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    string s;   cin >> s;
    auto [p, c] = compute_suffix(s);
    auto lcp = compute_lcp(p, c, s);
    int q = 1;
    while (q--) {
        int k;  cin >> k;
        cout << k_th_substring(p, lcp, s, k) << '\n';
    }

    return 0;
}
```