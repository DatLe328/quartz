```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

struct State {
    int len, link;
    map<char, int> next;
    int count;
    set<int> contain;   // use for search all string
};

struct SuffixAutomaton {
    vector<State> st;
    int last;
    long long distinct_sub_string;
    bool search_all_flag = false;   // enable this to use search_all

    SuffixAutomaton() {
        st.push_back({0, -1});
        last = 0;
        distinct_sub_string = 0;
    }
    void extend(char c, int index = -1) {
        int cur = (int)st.size();
        st.push_back({st[last].len + 1, -1});
        st[cur].count = 1;
        if (search_all_flag) st[cur].contain.insert(index); // Gắn index của chuỗi hiện tại

        int p = last;
        while (p != -1 && !st[p].next.count(c)) {
            st[p].next[c] = cur;
            p = st[p].link;
        }

        if (p == -1) {
            st[cur].link = 0;
        } else {
            int q = st[p].next[c];
            if (st[p].len + 1 == st[q].len) {
                st[cur].link = q;
            } else {
                int clone = (int)st.size();
                st.push_back({st[p].len + 1, st[q].link, st[q].next});
                if (search_all_flag) st[clone].contain = st[q].contain; // Clone tập `contain`

                while (p != -1 && st[p].next[c] == q) {
                    st[p].next[c] = clone;
                    p = st[p].link;
                }
                st[q].link = st[cur].link = clone;
            }
        }
        last = cur;
        int new_substrings = st[cur].len - st[st[cur].link].len;
        distinct_sub_string += new_substrings;
    }
    void build(const string& s) {
        for (char c : s) {
            extend(c);
        }

        // Compute count
        vector<int> order(st.size());
        iota(order.begin(), order.end(), 0);

        // Sắp xếp các trạng thái theo chiều dài giảm dần
        sort(order.begin(), order.end(), [&](int a, int b) {
            return st[a].len > st[b].len;
        });

        for (int i : order) {
            if (st[i].link != -1) {
                st[st[i].link].count += st[i].count;
            }
        }
    }
    void build_search_all(const vector<string>& v) {
        search_all_flag = true;
        for (int i = 0; i < (int)v.size(); i++) {
            last = 0; // Reset trạng thái cho mỗi chuỗi
            for (char c : v[i]) {
                extend(c, i);
            }
            extend('#', -1); // Ký tự phân cách đặc biệt, không thuộc chuỗi nào
        }

        // Truyền thông tin contain qua suffix link
        vector<int> order(st.size());
        iota(order.begin(), order.end(), 0);
        sort(order.begin(), order.end(), [&](int a, int b) {
            return st[a].len > st[b].len;
        });

        for (int i : order) {
            if (st[i].link != -1) {
                st[st[i].link].contain.insert(
                    st[i].contain.begin(), st[i].contain.end());
            }
        }
    }
    int search_all(const string& s) {
        // a = ["aaaa", "babbbb", "aaaa"]
        // q a  -> 3   
        // q aa -> 2    "aaaa", "aaaa"
        assert(search_all_flag);
        int cur = 0;
        for (char c : s) {
            if (!st[cur].next.count(c)) {
                return 0; // Không tìm thấy substring
            }
            cur = st[cur].next[c];
        }
        return st[cur].contain.size();
    }

    int count_occurrences(const string& s) {
        int cur = 0;
        for (char c : s) {
            if (!st[cur].next.count(c)) {
                return 0; // Không tìm thấy substring
            }
            cur = st[cur].next[c];
        }
        return st[cur].count;
    }

    long long get_diff_strings(){
        long long tot = 0;
        for(int i = 1; i < (int)st.size(); i++) {
            tot += st[i].len - st[st[i].link].len;
        }
        return tot;
    }
    int lcs(string s) {
        int n = (int)st.size();
        int p = 0, len = 0, best = 0;
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
            }
        }
        return best;
    }
    vector<int> LCS;
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
            LCS[i] = min(LCS[i], match[i]);
        }
    }
    int lcs_n(vector<string> s) { 
        int n = (int)st.size();
        LCS.assign(n, 1e9);
        int ans = 0;
        for (int i = 0; i < (int)s.size(); i++) {
            lcs_match(s[i]);
        }
        for (int i = 0; i < n; i++) {
            ans = max(ans, LCS[i]);
        }
        return ans;
    }
};
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int tt; cin >> tt;
    while (tt--) {
        int n;  cin >> n;
        n--;
        string s;   cin >> s;
        vector<string> res(n);
        for (int i = 0; i < n; i++) {
            cin >> res[i];
        }
        SuffixAutomaton sa;
        sa.build(s);
        cout << sa.lcs_n(res) << '\n';
    }

    return 0;
}
```