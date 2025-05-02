```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
 
const int MAX_N = 2e5 + 1;
 
struct tdata {
	ll sum;
	tdata *l, *r;
	tdata() : l(NULL), r(NULL), sum(0) {};
	tdata(tdata* left, tdata* right, ll val) : l(left), r(right), sum(val) {}
};
 
tdata* ver[MAX_N];
int n, q, cur, arr[MAX_N];
 
void build(tdata* node, int l, int r) {
	if (l == r) {
		node->sum = arr[l];
		return;
	}
	node->l = new tdata();
	node->r = new tdata();
	int mid = l + r >> 1;
	build(node->l, l, mid);
	build(node->r, mid + 1, r);
	node->sum = node->l->sum + node->r->sum;
}
 
void update(tdata* node, int l, int r, int i, int val) {
	if (l == r) {
		node->sum = val;
		return;
	}
	int mid = l + r >> 1;
	if (mid >= i) {
		tdata* left = node->l;
		node->l = new tdata(left->l, left->r, left->sum);
		update(node->l, l, mid, i, val);
	}
	else {
		tdata* right = node->r;
		node->r = new tdata(right->l, right->r, right->sum);
		update(node->r, mid + 1, r, i, val);
	}
	node->sum = node->l->sum + node->r->sum;
}
 
ll query(tdata* node, int l, int r, int u, int v) {
	if (l > v || r < u)
		return 0;
	if (l >= u && r <= v)
		return node->sum;
	int mid = l + r >> 1;
	ll left = query(node->l, l, mid, u, v);
	ll right = query(node->r, mid + 1, r, u, v);
	return left + right;
}
 
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);
 
	cin >> n >> q;
	for (int i = 1; i <= n; i++)
		cin >> arr[i];
	cur = 1;
	ver[1] = new tdata();
	build(ver[cur], 1, n);
	while (q--) {
		int x, k;	cin >> x >> k;
		if (x == 1) {
			int u, v;	cin >> u >> v;
			update(ver[k], 1, n, u, v);
		}
		else if (x == 2) {
			int u, v;	cin >> u >> v;
			cout << query(ver[k], 1, n, u, v) << '\n';
		}
		else {
			ver[++cur] = new tdata(ver[k]->l, ver[k]->r, ver[k]->sum);
		}
	}
 
	return 0;
}

```