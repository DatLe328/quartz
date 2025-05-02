```cpp
vector<int> manacher_odd(string s) {
    int n = s.size();
    s = "$" + s + "^";
    vector<int> p(n + 2);
    int l = 1, r = 1;
    for (int i = 1; i <= n; i++) {
        p[i] = max(0, min(r - i, p[l + (r - i)]));
        while (s[i - p[i]] == s[i + p[i]]) {
            p[i]++;
        }
        if(i + p[i] > r) {
            l = i - p[i], r = i + p[i];
        }
    }
    return vector<int>(begin(p) + 1, end(p) - 1);
}
vector<int> manacher(string s) {
    string t;
    for(auto c: s) {
        t += string("#") + c;
    }
    auto res = manacher_odd(t + "#");
    return vector<int>(begin(res) + 1, end(res) - 1);
}

string longest_palindrome(vector<int> p, string s) {
    int max_len = 0, center = 0;
    for (int i = 0; i < (int)p.size(); i++) {
        if (p[i] > max_len) {
            max_len = p[i];
            center = i;
        }
    }
    int start = (center - max_len) / 2;
    return s.substr(start + 1, max_len - 1);
}
```