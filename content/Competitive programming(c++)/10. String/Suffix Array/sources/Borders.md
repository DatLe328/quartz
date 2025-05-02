```cpp
#include<bits/stdc++.h>
#define ll long long int
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

void countSort(vector<int> &p, vector<int> &c) {
    int n = p.size();
    vector<int> cnt(n);
    for (auto x : c) {
        cnt[x]++;
    }
    vector<int> pos(n);
    pos[0] = 0;
    for (int x = 1; x < n; x++) {
        pos[x] = pos[x - 1] + cnt[x - 1];
    }
    vector<int> p_new(n);
    for (auto x : p) {
        p_new[pos[c[x]]] = x;
        pos[c[x]]++;
    }
    p = p_new;
}

pair<vector<int>, vector<int>> computeSuffixArray(string s) {
    s += "$";
    int n = s.size();
    vector<int> pos(n), class_(n);
    // pos[i]: the i-th item's index, i.e., s[pos[i]:] is the i-th item in suffix
    // array
    // class_[i]: the equivalance class of s[i]
    {
        // k = 0
        vector<pair<char, int>> a(n);
        for (int i = 0; i < n; i++) {
            a[i] = {s[i], i};
        }
        sort(a.begin(), a.end());
        for (int i = 0; i < n; i++) {
            pos[i] = a[i].second;
        }
        class_[pos[0]] = 0;
        for (int i = 1; i < n; i++) {
            class_[pos[i]] = class_[pos[i - 1]] + int(a[i - 1].first != a[i].first);
        }
    }
    int k = 0;
    while ((1 << k) < n && class_[pos[n - 1]] < n - 1) {  // for early termination
        // k -> k + 1
        for (int i = 0; i < n; i++) {
            // actually, this is the first counting sort of a radix sort
            //   class_[i] = class_[(pos[i] - (1 << k) + n) % n]
            pos[i] = (pos[i] - (1 << k) + n) % n;
        }
        countSort(pos, class_);
        vector<int> class_new(n);
        class_new[pos[0]] = 0;
        for (int i = 1; i < n; i++) {
            pair<int, int> prev = {class_[pos[i - 1]],
                                   class_[(pos[i - 1] + (1 << k)) % n]};
            pair<int, int> curr = {class_[pos[i]], class_[(pos[i] + (1 << k)) % n]};
            class_new[pos[i]] = class_new[pos[i - 1]] + int(prev != curr);
        }
        class_ = class_new;
        k++;
    }
    return {pos, class_};
}

vector<int> computeLCP(const vector<int> &p, const vector<int> &c,
                       const string &s) {
    int n = p.size();
    int k = 0;
    vector<int> lcp(n - 1);
    for (int i = 0; i < n - 1; i++) {
        int pi = c[i];  // pi >= 1 as "$" is always the smallest suffix string
        int j = p[pi - 1];
        // lcp[pi] = lcp(s[i:], s[j:])
        while (s[i + k] == s[j + k]) {
            k++;
        }
        lcp[pi - 1] = k;  // shift to left by one
        k = max(k - 1, 0);
    }
    return lcp;
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    string s;   cin >> s;
    auto res = computeSuffixArray(s);
    auto lcp = computeLCP(res.first, res.second, s);

    ll n = lcp.size();
    ll ans = n * (n + 1) / 2;   // empty borders for all substrings
    stack<pair<int, int>> mono_stack;
    for (int i = 0; i <= n; i++) {
        int pos = i, item = (i == n ? 0 : lcp[i]);
        while (mono_stack.size() && mono_stack.top().second >= item) {
            int last_pos = mono_stack.top().first, last_item = mono_stack.top().second;
            mono_stack.pop();
            if (last_item > item) {
                int threshold = mono_stack.empty() ? item : max(item, mono_stack.top().second);
                ll pos_diff = i - last_pos + 1;
                // border length: threshold + 1, threshold + 2, ..., last_item
                // lcp index: last_pos, last_pos + 1, ..., i - 1
                ans += pos_diff * (pos_diff - 1) / 2 * (last_item - threshold);
            }
            pos = last_pos;
        }
        mono_stack.push({pos, item});
    }
    cout << ans;

    return 0;
}
```