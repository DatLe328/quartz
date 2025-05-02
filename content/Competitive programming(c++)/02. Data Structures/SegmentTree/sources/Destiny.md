# Solution 1: Wavelet tree
```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 3e5 + 1;
const int base = 1e9;
int a[MAX_N], valmid;
bool cmp(int x) { return x <= valmid; }
struct wavelet_tree
{
    int low, high;
    wavelet_tree *L, *R;
    vector <int> tmp;
 
    wavelet_tree(int *u, int *v, int x, int y)
    {
        low = x; high = y;
        if (low == high || u >= v) return;
        valmid = (low + high)/2;
 
        tmp.reserve(v-u+1);
        tmp.push_back(0);
        for (int* i=u; i != v; i++)
            tmp.push_back(tmp.back() + (*i <= valmid));
 
        int *p = stable_partition(u, v, cmp);
        L = new wavelet_tree(u, p, x, valmid);
        R = new wavelet_tree(p, v, valmid+1, y);
    }
 
    int occur(int l, int r, int num)
    {
        if (r-l+1 < num) return base;
        if (low == high)
        {
            if (r-l+1 >= num) return low;
            return base;
        }
        int lb = tmp[l-1], rb = tmp[r];
        return min(this->L->occur(lb+1, rb, num), this->R->occur(l-lb, r-rb, num));
    }
};

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int i,n,k,j,q,l,r;
	cin >> n >> q;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
	wavelet_tree T(a+1, a+n+1, 1, n);
	while(q--){
		cin >> l >> r >> k;
        k = (r-l+1)/k + 1;
        int ans = T.occur(l, r, k);
        cout << (ans == base ? -1 : ans) << '\n';
	}

    return 0;
}
```
# Solution 2: Randomize and Mo'algorithm
```cpp
#include <bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

#define ll long long

ll start = chrono::steady_clock::now().time_since_epoch().count();
mt19937_64 rng(start);

int rand(int l, int r) {
    return uniform_int_distribution<int>(l, r)(rng);
}

const int MAX_N = 3e5 + 1;
const int D = 500;

struct query {
    int l, r, id, k;
    query() {}
    query(int l, int r, int id, int k) : l(l), r(r), id(id), k(k) {}
    bool operator< (query b) {
        query a = *this;
        return make_pair(a.l / D, a.r) < make_pair(b.l / D, b.r);
    }
};
query qs[MAX_N];
int a[MAX_N], cnt[MAX_N], ans[MAX_N];
int n, q;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> q;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    for (int i = 1; i <= q; i++) {
        int l, r, k;    cin >> l >> r >> k;
        qs[i] = query(l, r, i, k);
    }
    sort(qs + 1, qs + 1 + q);
    int curL = 1, curR = 0;

    for (int i = 1; i <= q; i++) {
        auto [l, r, id, k] = qs[i];
        while (curL < l) cnt[a[curL++]]--;
        while (curL > l) cnt[a[--curL]]++;
        while (curR > r) cnt[a[curR--]]--;
        while (curR < r) cnt[a[++curR]]++;
        int res = INT_MAX;
        for (int attemp = 0; attemp < 100; attemp++) {
            int rnd = a[l + rand(0, r - l)];
            if (cnt[rnd] > (r - l + 1) / k) {
                res = min(res, rnd);
            }
        }
        ans[id] = (res == INT_MAX ? -1 : res);
    }
    for (int i = 1; i <= q; i++) {
        cout << ans[i] << '\n';
    }
    return 0;
}

```