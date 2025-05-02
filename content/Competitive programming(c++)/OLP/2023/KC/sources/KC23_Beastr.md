```cpp
#include<bits/stdc++.h>
#include <pthread.h>
using namespace std;

struct Node {
	vector<int> cnt;
	Node() : cnt(26, 0) {}
};
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	int n, q;	cin >> n >> q;
	string s;	cin >> s;
	vector<Node> cnt(n + 1);
	for (int i = 1; i <= n; i++) {
		int v = int(s[i - 1] - 'a');
		for (int j = 0; j < 26; j++) {
			if (j == v) {
				cnt[i].cnt[j]++;
			}
			cnt[i].cnt[j] += cnt[i - 1].cnt[j];
		}
	}
	while (q--) {
		int l, r;	cin >> l >> r;
		l++, r++;
		int odd = 0;
		for (int i = 0; i < 26; i++) {
			int res = cnt[r].cnt[i] - cnt[l - 1].cnt[i];
			if (res & 1) {
				odd++;
			}
		}
		if (odd < 2) {
			cout << 0 << '\n';
		}
		else {
			cout << odd / 2 << '\n';
		}
	}

	return 0;
}

```