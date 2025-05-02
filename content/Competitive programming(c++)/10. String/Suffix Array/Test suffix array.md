```cpp
struct Test {
    void count_sort(vector<int>& p, vector<int>& c) {
        int n = int(p.size());
        vector<int> cnt(n);
        for (int x : c) {
            cnt[x]++;
        }
        vector<int> pos(n);
        pos[0] = 0;
        for (int i = 1; i < n; i++) {
            pos[i] = pos[i - 1] + cnt[i - 1];
        }
        vector<int> p_new(n);
        for (int x : p) {
            p_new[pos[c[x]]] = x;
            pos[c[x]]++;
        }
        p = p_new;
    }

    pair<vector<int>, vector<int>> compute_suffix(string s) {
        s += '$';
        int n = s.size();
        vector<int> p(n), c(n);
        {
            vector<pair<char, int>> a(n);
            for (int i = 0; i < n; i++) {
                a[i] = {s[i], i};
            }
            sort(a.begin(), a.end());
            for (int i = 0; i < n; i++) {
                p[i] = a[i].second;
            }
            c[p[0]] = 0;
            for (int i = 1; i < n; i++) {
                c[p[i]] = c[p[i - 1]] + int(a[i - 1].first != a[i].first);
            }
        }
        int k = 0;

        while ((1 << k) < n && c[p[n - 1]] < n - 1) {
            for (int i = 0; i < n; i++) {
                p[i] = (p[i] - (1 << k) + n) % n;
            }
            count_sort(p, c);
            vector<int> c_new(n);
            c_new[p[0]] = 0;
            for (int i = 1; i < n; i++) {
                pair<int, int> prev = { c[p[i - 1]], c[(p[i - 1] + (1 << k)) % n]};
                pair<int, int> cur = { c[p[i]], c[(p[i] + (1 << k)) % n]};
                c_new[p[i]] = c_new[p[i - 1]] + int(prev != cur);
            }
            c = c_new;
            k++;
        }
        return {p, c};
    }
    vector<int> compute_lcp(vector<int> p, vector<int> c, string s) {
        int n = s.size();
        int k = 0;
        vector<int> lcp(n);
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
    Test(string s = "ababba") {
        auto [p, c] = compute_suffix(s);
        auto lcp = compute_lcp(p, c, s);
        cerr << "Expected value\n";
        debug(p);
        debug(c);
        debug(lcp);
        cerr << '\n';
    }
};
```