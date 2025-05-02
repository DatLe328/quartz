```cpp
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
int compare(int i, int j, int len) {
    // compare s[i...i+l-1] and s[j...j+l-1]
    // greatest k such that 2^k <= l
    // compare with i and j start from 1
    // want to compare with i and j start from 1 remove -1
    int k = 31 - __builtin_clz(len);
    pair<int, int> a = {cc[k][i - 1], cc[k][(i + len - (1 << k)) - 1]};
    pair<int, int> b = {cc[k][j - 1], cc[k][(j + len - (1 << k)) - 1]};
    return a == b ? 0 : a < b ? -1 : 1;
}
void substring_sort(vector<pair<int, int>>& a) {
    sort(a.begin(), a.end(), [&] (pair<int, int> l, pair<int, int> r) -> int {
        auto [x1, y1] = l;
        auto [x2, y2] = r;
        int len1 = y1 - x1 + 1, len2 = y2 - x2 + 1;
        int min_len = min(len1, len2);
        int cmp = compare(x1, x2, min_len);
        if (cmp != 0) return cmp < 0;
        if (len1 != len2) return len1 < len2;
        return l < r;
    });
}
int lower_bound(vector<int>& p, string& s, string& t) {
    int n = (int)s.size();
    int m = (int)t.size();
    int l = 1, r = n, ret = -1; // Giới hạn chỉ số hợp lệ trong p
    while (l <= r) {
        int mid = (l + r) / 2;
        int cur = min(m, n - p[mid]);
        string res = s.substr(p[mid], cur);
        if (t == res) {
            ret = mid;
            r = mid - 1;
        }
        else if (t > res) {
            l = mid + 1;
        }
        else {
            r = mid - 1;
        }
    }
    return ret;
}

int upper_bound(vector<int>& p, string& s, string& t) {
    int n = (int)s.size();
    int m = (int)t.size();
    int l = 1, r = n, ret = -1;
    while (l <= r) {
        int mid = (l + r) / 2;
        int cur = min(m, n - p[mid]);
        string res = s.substr(p[mid], cur);
        if (t == res) {
            ret = mid;
            l = mid + 1;
        }
        else if (t > res) {
            l = mid + 1;
        }
        else {
            r = mid - 1;
        }
    }
    return ret;
}
```