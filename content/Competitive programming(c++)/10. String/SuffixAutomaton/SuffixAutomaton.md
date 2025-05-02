```cpp
struct State {
    int len, link;
    map<char, int> next;
    int count, first_pos;
    unordered_set<int> pos;
};
struct SuffixAutomaton {
    vector<State> st;
    int last;
    long long dict;
    vector<long long> cnt;

    SuffixAutomaton() {
        st.push_back({0, -1});
        last = 0;
        dict = 0;
    }
    void extend(char c, int pos = -1) {
        int cur = (int)st.size();
        st.push_back({st[last].len + 1, -1, {}, 1, pos});
        if (pos != -1) st.back().pos.insert(pos);
        int p = last;

        while (p != -1 && !st[p].next.count(c)) {
            st[p].next[c] = cur;
            p = st[p].link;
        }
        if (p == -1) {
            st[cur].link = 0;
        }
        else {
            int q = st[p].next[c];
            if (st[p].len + 1 == st[q].len) {
                st[cur].link = q;
            }
            else {
                int clone = (int)st.size();
                st.push_back({st[p].len + 1, st[q].link, st[q].next, 0, st[q].first_pos});
                if (pos != -1) st.back().pos = st[q].pos;

                while (p != -1 && st[p].next[c] == q) {
                    st[p].next[c] = clone;
                    p = st[p].link;
                }   
                st[q].link = st[cur].link = clone;
            }
        }
        last = cur;
        dict += st[cur].len - st[st[cur].link].len;
    }
    void build(string s) {
        for (size_t i = 0; i < s.size(); i++) {
            extend(s[i], i + 1);
        }
        vector<int> order(st.size());
        iota(order.begin(), order.end(), 0);
        sort(order.begin(), order.end(), [&] (int i, int j) { return st[i].len > st[j].len; });
        for (int i : order) {
            if (st[i].link != -1) {
                st[st[i].link].count += st[i].count;
            }
        }
    }
    void build_search_exist(vector<string> v) {
        for (int i = 0; i < (int)v.size(); i++) {
            last = 0;
            for (char c : v[i]) {
                extend(c, i);
            }
        }

        vector<int> order(st.size());
        iota(order.begin(), order.end(), 0);
        sort(order.begin(), order.end(), [&] (int i, int j) { return st[i].len > st[j].len; });
        for (int i : order) {
            if (st[i].link != -1) {
                st[st[i].link].pos.insert(st[i].pos.begin(), st[i].pos.end());
            }
        }
    }
    int count_exist(string s) {
        // a = ["aaaa", "babbbb", "aaaa"]
        // q a  -> 3   
        // q aa -> 2    "aaaa", "aaaa"
        int p = 0;
        for (char c : s) {
            if (!st[p].next.count(c)) return 0;
            p = st[p].next[c];
        }
        return st[p].pos.size();
    }
    int count_occ(string s) {
        int p = 0;
        for (char c : s) {
            if (!st[p].next.count(c)) return 0;
            p = st[p].next[c];
        }
        return st[p].count;
    }
    int find_first(string s) {
        int p = 0;
        for (char c : s) {
            if (!st[p].next.count(c)) return -1;
            p = st[p].next[c];
        }
        return st[p].first_pos - s.size() + 1;
    }

    // find lexicographically minimal lcs
    string find_lcs(string s) {
        int p = 0, len = 0, best = 0, end_pos = -1;
        string lcs = "";

        for (int i = 0; i < (int)s.size(); i++) {
            while (p && !st[p].next.count(s[i])) {
                p = st[p].link; 
                len = st[p].len;
            }
            if (st[p].next.count(s[i])) {
                p = st[p].next[s[i]];
                len++;
            }

            if (len > best) {
                best = len;
                end_pos = i;
                lcs = s.substr(end_pos - best + 1, best); 
            }
            else if (len == best) {
                string current_lcs = s.substr(i - len + 1, len);
                if (current_lcs < lcs) { 
                    lcs = current_lcs;
                }
            }
        }

        return lcs;
    }

    vector<int> lcs;
    void lcs_match(string s) {
        int n = (int)st.size();
        vector<int> match(n);
        int p = 0, len = 0;
        for (int i = 0; i < (int)s.size(); i++) {
            while (p && !st[p].next.count(s[i])) {
                p = st[p].link;
                len = st[p].len;
            }
            if (st[p].next.count(s[i])) {
                p = st[p].next[s[i]];
                len++;
            }
            match[p] = max(match[p], len);
        }
        for (int i = n - 1; i >= 0; i--) {
            match[i] = max(match[i], match[st[i].link]);
        }
        for (int i = 0; i < n; i++) {
            lcs[i] = min(lcs[i], match[i]);
        }
    }
    int lcs_n(vector<string> s) { 
        int n = (int)st.size();
        lcs.assign(n, 1e9);
        int ans = 0;
        for (int i = 0; i < (int)s.size(); i++) {
            lcs_match(s[i]);
        }
        for (int i = 0; i < n; i++) {
            ans = max(ans, lcs[i]);
        }
        return ans;
    }
    
    // string s is the current string Suffix is holding
    // becareful this function is very slow
    void print_distinct_substrings(string s) {
        set<string> substrings;
        for (int i = 0; i < (int)st.size(); i++) {
            int length = (int)st[i].len;
            int link_length = st[i].link == -1 ? 0 : st[st[i].link].len;
            for (int j = link_length + 1; j <= length; j++) {
                substrings.insert(s.substr(st[i].first_pos - j, j));
            }
        }
        for (const auto& str : substrings) {
            cout << str << '\n';
        }
    }
    vector<long long> count_distinct_by_length(int n) {
        vector<long long> result(n + 1, 0);
        
        for (int i = 1; i < (int)st.size(); i++) {
            int length = (int)st[i].len;
            int link_length = st[st[i].link].len;
            for (int len = link_length + 1; len <= length; len++) {
                result[len]++;
            }
        }
        return result;
    }
    long long dfs(int p) {
        if (cnt[p] != -1) return cnt[p];
        long long ret = 1; 
        for (auto [c, next_state] : st[p].next) {
            ret += dfs(next_state);
        }
        return cnt[p] = ret;
    }
 
    void kth(vector<char>& ans, int p, long long& k) {
        if (k <= 0) return;
        for (auto [c, i] : st[p].next) {
            long long tmp = dfs(i);
            if (k <= tmp) {
                ans.push_back(c);
                k--;
                kth(ans, i, k);
                return;
            } else {
                k -= tmp;
            }
        }
    }
    string find_kth(long long k) {
        cnt.resize(st.size(), -1);
        vector<char> ans;
        kth(ans, 0, k);
        return string(ans.begin(), ans.end());
    }
};
```

```cpp
struct State {
    int len, link;
    unordered_map<char, int> next;
    int count, first_pos;
    unordered_set<int> pos;
};
struct SuffixAutomaton {
    vector<State> st;
    int last;
    long long dict;

    SuffixAutomaton() {
        st.push_back({0, -1});
        last = 0;
        dict = 0;
    }
    void extend(char c, int pos = -1) {
        int cur = (int)st.size();
        st.push_back({st[last].len + 1, -1, {}, 1, pos});
        if (pos != -1) st.back().pos.insert(pos);
        int p = last;

        while (p != -1 && !st[p].next.count(c)) {
            st[p].next[c] = cur;
            p = st[p].link;
        }
        if (p == -1) {
            st[cur].link = 0;
        }
        else {
            int q = st[p].next[c];
            if (st[p].len + 1 == st[q].len) {
                st[cur].link = q;
            }
            else {
                int clone = (int)st.size();
                st.push_back({st[p].len + 1, st[q].link, st[q].next, 0, st[q].first_pos});
                if (pos != -1) st.back().pos = st[q].pos;

                while (p != -1 && st[p].next[c] == q) {
                    st[p].next[c] = clone;
                    p = st[p].link;
                }   
                st[q].link = st[cur].link = clone;
            }
        }
        last = cur;
        dict += st[cur].len - st[st[cur].link].len;
    }
    void build(string s) {
        for (size_t i = 0; i < s.size(); i++) {
            extend(s[i], i + 1);
        }
        vector<int> order(st.size());
        iota(order.begin(), order.end(), 0);
        sort(order.begin(), order.end(), [&] (int i, int j) { return st[i].len > st[j].len; });
        for (int i : order) {
            if (st[i].link != -1) {
                st[st[i].link].count += st[i].count;
            }
        }
    }
    void build_search_exist(vector<string> v) {
        for (int i = 0; i < (int)v.size(); i++) {
            last = 0;
            for (char c : v[i]) {
                extend(c, i);
            }
        }

        vector<int> order(st.size());
        iota(order.begin(), order.end(), 0);
        sort(order.begin(), order.end(), [&] (int i, int j) { return st[i].len > st[j].len; });
        for (int i : order) {
            if (st[i].link != -1) {
                st[st[i].link].pos.insert(st[i].pos.begin(), st[i].pos.end());
            }
        }
    }
    int count_exist(string s) {
        // a = ["aaaa", "babbbb", "aaaa"]
        // q a  -> 3   
        // q aa -> 2    "aaaa", "aaaa"
        int p = 0;
        for (char c : s) {
            if (!st[p].next.count(c)) return 0;
            p = st[p].next[c];
        }
        return st[p].pos.size();
    }
    int count_occ(string s) {
        int p = 0;
        for (char c : s) {
            if (!st[p].next.count(c)) return 0;
            p = st[p].next[c];
        }
        return st[p].count;
    }
    int find_first(string s) {
        int p = 0;
        for (char c : s) {
            if (!st[p].next.count(c)) return -1;
            p = st[p].next[c];
        }
        return st[p].first_pos - s.size() + 1;
    }

    // find lexicographically minimal lcs
    string find_lcs(string s) {
        int n = (int)st.size();
        int p = 0, len = 0, best = 0, end_pos = -1;
        string lcs = "";

        for (int i = 0; i < (int)s.size(); i++) {
            while (p && !st[p].next.count(s[i])) {
                p = st[p].link; 
                len = st[p].len;
            }
            if (st[p].next.count(s[i])) {
                p = st[p].next[s[i]];
                len++;
            }

            if (len > best) {
                best = len;
                end_pos = i;
                lcs = s.substr(end_pos - best + 1, best); 
            }
            else if (len == best) {
                string current_lcs = s.substr(i - len + 1, len);
                if (current_lcs < lcs) { 
                    lcs = current_lcs;
                }
            }
        }

        return lcs;
    }

    vector<int> lcs;
    void lcs_match(string s) {
        int n = (int)st.size();
        vector<int> match(n);
        int p = 0, len = 0;
        for (int i = 0; i < (int)s.size(); i++) {
            while (p && !st[p].next.count(s[i])) {
                p = st[p].link;
                len = st[p].len;
            }
            if (st[p].next.count(s[i])) {
                p = st[p].next[s[i]];
                len++;
            }
            match[p] = max(match[p], len);
        }
        for (int i = n - 1; i >= 0; i--) {
            match[i] = max(match[i], match[st[i].link]);
        }
        for (int i = 0; i < n; i++) {
            lcs[i] = min(lcs[i], match[i]);
        }
    }
    int lcs_n(vector<string> s) { 
        int n = (int)st.size();
        lcs.assign(n, 1e9);
        int ans = 0;
        for (int i = 0; i < (int)s.size(); i++) {
            lcs_match(s[i]);
        }
        for (int i = 0; i < n; i++) {
            ans = max(ans, lcs[i]);
        }
        return ans;
    }
    
    // string s is the current string Suffix is holding
    void print_distinct_substrings(string s) {
        set<string> substrings;
        for (int i = 0; i < st.size(); i++) {
            int length = st[i].len;
            int link_length = st[i].link == -1 ? 0 : st[st[i].link].len;
            for (int j = link_length + 1; j <= length; j++) {
                substrings.insert(s.substr(st[i].first_pos - j, j));
            }
        }
        for (const auto& str : substrings) {
            cout << str;
        }
    }
    vector<long long> count_distinct_by_length(int n) {
        vector<long long> result(n + 1, 0);
        
        for (int i = 1; i < st.size(); i++) {
            int length = st[i].len;                
            int link_length = st[st[i].link].len;
            for (int len = link_length + 1; len <= length; len++) {
                result[len]++;
            }
        }
        return result;
    }
};
```