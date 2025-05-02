# Solution1 : Mergesort tree
```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 1e5 + 1;
pair<int, int> a[MAX_N];
int idx[MAX_N];
vector<int> st[4 * MAX_N];
int n, q;

void build(int id, int l, int r) {
	if (l == r) {
		st[id].push_back(a[l].second);
		return;
	}
	int mid = (l + r) / 2;
	build(id * 2, l, mid);
	build(id * 2 + 1, mid + 1, r);
	merge(st[id * 2].begin(), st[id * 2].end(), st[id * 2 + 1].begin(), st[id * 2 + 1].end(), back_inserter(st[id]));
}

int query(int id, int l, int r, int u, int v, int k) {
	if (l == r) {
		return st[id].back();
	}
	int f = upper_bound(st[id * 2].begin(), st[id * 2].end(), v) - 
			lower_bound(st[id * 2].begin(), st[id * 2].end(), u);
	int mid = (l + r) / 2;
	if (f >= k) {
		return query(id * 2, l, mid, u, v, k);
	}
	return query(id * 2 + 1, mid + 1, r, u, v, k - f);
}
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	cin >> n >> q;
	for (int i = 1; i <= n; i++) {
		cin >> a[i].first;
		a[i].second = i;
		idx[i] = a[i].first;
	}
	sort(a + 1, a + 1 + n);
	build(1, 1, n);
	while (q--) {
		int l, r, k;	cin >> l >> r >> k;
		int f = query(1, 1, n, l, r, k);
		cout << idx[f] << '\n';
	}

	return 0;
}
```
# Solution 2: Persistent tree
```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAX_N = 2e5 + 1;

struct tdata {
    int cnt; // Số lượng phần tử trong khoảng
    tdata *l, *r;
    tdata() : cnt(0), l(NULL), r(NULL) {}
    tdata(tdata* left, tdata* right, int count) : l(left), r(right), cnt(count) {}
};

tdata* ver[MAX_N];
int n, q, arr[MAX_N];

vector<int> compressed; // Mảng lưu giá trị đã nén

void build(tdata* node, int l, int r) {
    if (l == r) {
        node->cnt = 0;
        return;
    }
    node->l = new tdata();
    node->r = new tdata();
    int mid = (l + r) >> 1;
    build(node->l, l, mid);
    build(node->r, mid + 1, r);
}

void update(tdata* prev, tdata* node, int l, int r, int idx) {
    if (l == r) {
        node->cnt = prev->cnt + 1;
        return;
    }
    int mid = (l + r) >> 1;
    if (idx <= mid) {
        node->r = prev->r;
        node->l = new tdata(prev->l->l, prev->l->r, prev->l->cnt);
        update(prev->l, node->l, l, mid, idx);
    } else {
        node->l = prev->l;
        node->r = new tdata(prev->r->l, prev->r->r, prev->r->cnt);
        update(prev->r, node->r, mid + 1, r, idx);
    }
    node->cnt = node->l->cnt + node->r->cnt;
}

int kth_query(tdata* u, tdata* v, int l, int r, int k) {
    if (l == r) return l;
    int mid = (l + r) >> 1;
    int count_in_left = v->l->cnt - u->l->cnt;
    if (k <= count_in_left) {
        return kth_query(u->l, v->l, l, mid, k);
    } else {
        return kth_query(u->r, v->r, mid + 1, r, k - count_in_left);
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> q;
    for (int i = 1; i <= n; i++) cin >> arr[i];

    // Nén giá trị
    compressed = vector<int>(arr + 1, arr + n + 1);
    sort(compressed.begin(), compressed.end());
    compressed.erase(unique(compressed.begin(), compressed.end()), compressed.end());

    for (int i = 1; i <= n; i++) {
        arr[i] = lower_bound(compressed.begin(), compressed.end(), arr[i]) - compressed.begin() + 1;
    }

    // Xây dựng cây
    ver[0] = new tdata();
    build(ver[0], 1, compressed.size());

    for (int i = 1; i <= n; i++) {
        ver[i] = new tdata();
        update(ver[i - 1], ver[i], 1, compressed.size(), arr[i]);
    }

    // Truy vấn
    while (q--) {
        int l, r, k;
        cin >> l >> r >> k;
        int index = kth_query(ver[l - 1], ver[r], 1, compressed.size(), k);
        cout << compressed[index - 1] << '\n';
    }

    return 0;
}

```