```cpp
#include<bits/stdc++.h>
using namespace std;
 
#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif
 
struct Trie {
	struct Node {
		vector<int> child;
		int cnt, exist;
		Node() : child(26, -1), cnt(0), exist(0) {}
	};
	vector<Node> nodes;
	Trie() :  nodes(1) {}
	void add_string(string s) {
		int v = 0;
		int n = int(s.size());
		for (int i = 0; i < n; i++) {
			int c = int(s[i] - 'a');
			if (nodes[v].child[c] == -1) {
				nodes[v].child[c] = nodes.size();
				nodes.emplace_back();
			}
			v = nodes[v].child[c];
			nodes[v].cnt++;
		}
		nodes[v].exist++;
	}
 
    void delete_string(string s) {
        if (find_string(s) == false) return;
        int pos = 0;
		int n = int(s.size());
        for (int i = 0; i < n; i++) {
			int c = int(s[i] - 'a');
 
            int tmp = nodes[pos].child[c];
            nodes[tmp].cnt--;
            if (nodes[tmp].cnt == 0) {
                nodes[pos].child[c] = -1;
                return;
            }
 
            pos = tmp;
        }
        nodes[pos].exist--;
    }
 
 
    bool find_string(string s) {
        int pos = 0;
        for (auto f : s) {
            int c = f - 'a';
            if (nodes[pos].child[c] == -1) return false;
            pos = nodes[pos].child[c];
        }
        return (nodes[pos].exist != 0);
    }
};
 
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);
 
	string s;	cin >> s;
	int k;	cin >> k;
	Trie t;
	for (int i = 0; i < k; i++) {
		string res;	cin >> res;
		t.add_string(res);
        t.delete_string(res);
        t.add_string(res);
	}
	using ll = int64_t;
	int n = s.size();
	ll MOD = 1e9 + 7;
	vector<ll> dp(n + 1, 0);
	dp[n] = 1;
	for (int i = n - 1; i >= 0; i--) {
		int v = 0;
		for (int j = i; j < n; j++) {
			int c = int(s[j] - 'a');
			if (t.nodes[v].child[c] == -1) {
				break;
			}
			v = t.nodes[v].child[c];
			if (t.nodes[v].exist) {
				dp[i] = (dp[i] + dp[j + 1]) % MOD;
			}
		}
	}
	cout << dp[0];
 
	return 0;
}

```