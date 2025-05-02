```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

void count_sort(vector<int>& p, vector<int>& c){
    int n = p.size();
    vector<int> t(n), cnt(n), pos(n);
    for (int i = 0; i < n; i++){
        cnt[c[p[i]]]++;
    }
    for (int i = 1; i < n; i++){
        pos[i] = pos[i - 1] + cnt[i - 1];
    }
    for (int i = 0; i < n; i++){
        t[pos[c[p[i]]]++] = p[i];
    }
    t.swap(p);
}
 
vector<int> build_suffix_array(string& s){
    s += 32;
    int n = s.length();
    vector<int> c(n), p(n), t(n);
    for (int i = 0; i < n; i++){
        p[i] = i;
    }
    sort(p.begin(), p.end(), [&](int lf, int rt){return (s[lf] < s[rt]);});
    for (int i = 1; i < n; i++){
        c[p[i]] = c[p[i - 1]] + (s[p[i]] != s[p[i - 1]]);
    }
    for (int k = 0; (1 << k) <= n; k++){
        int len = (1 << k);
        for (int i = 0; i < n; i++){
            p[i] = (p[i] - len + n) % n;
        }
        count_sort(p, c);
        for (int j = 1; j < n; j++){
            bool f = (c[p[j]] != c[p[j - 1]] || c[(p[j] + len) % n] != c[(p[j - 1] + len) % n]);
            t[p[j]] = t[p[j - 1]] + f;
        }
        c.swap(t);
    }
    return p;
}
 
pair<vector<int>, vector<int>> build_lcp(const vector<int>& p, string& s){
    int n = s.length();
    vector<int> rank(n);
    for (int i = 0; i < n; i++){
        rank[p[i]] = i;
    }
    vector<int> lcp(n - 1);
    int k = 0;
    for (int i = 0; i < (n - 1); i++){
        while (s[i + k] == s[p[rank[i] - 1] + k]){
            k++;
        }
        lcp[rank[i] - 1] = k;
        k -= (k != 0);
    }
    return {lcp, rank};
} 
 
class SparseTable{
    private:
        vector<int> a;
        int n;
        vector<vector<int>> st;
 
        void make_sparse_table(){
            n = a.size();
            st.resize(n);
            for (int i = 0; i < n; i++){
                st[i].push_back(a[i]);
            }
            for (int len = 2; len <= n; len *= 2){
                for (int l = 0; l + len - 1 < n; l++){
                    int m = l + len / 2;
                    st[l].push_back(min(st[l].back(), st[m].back()));
                }
            }
        }
 
    public:
        SparseTable(const vector<int>& v){
            a = v;
            make_sparse_table();
        }
 
        int rmq(int l, int r){ // minima
            if (l > r){
                swap(l, r);
            }
            r--;
            int power = log2(r - l + 1); // log len
            return min(st[l][power], st[r - (1 << power) + 1][power]);
        }
};
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    string s;   cin >> s;
    auto p = build_suffix_array(s);
    auto [lcp, rank] = build_lcp(p, s);

    int m;  cin >> m;
    SparseTable st(lcp);
    function<int(pair<int, int>&, pair<int, int>&)> get_lcp = [&](pair<int, int>& lf, pair<int, int>& rt){
        if (lf.first == rt.first){
            return min(lf.second - lf.first, rt.second - rt.first) + 1;
        }
        return st.rmq(rank[lf.first - 1], rank[rt.first - 1]);
    };
    
    return 0;
}
```