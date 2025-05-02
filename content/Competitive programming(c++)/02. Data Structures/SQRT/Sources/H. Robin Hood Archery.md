We can keep the count of appearances of each element using an array in O(1) time. Sort the queries into blocks of size Nsqrt(N). Keep updating the boundaries of the current segment and the total count of elements that appear an odd number of times. Sheriff can tie if there is no odd appearance.

Time complexity — O((N+Q)sqrt(N))
https://codeforces.com/contest/2014/problem/H
```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 2e5 + 1;
const int D = 500;

struct query {
    int l, r, id;
    query() {}
    query(int l, int r, int id) : l(l), r(r), id(id) {}
    bool operator< (query b) {
        query a = *this;
        return make_pair(a.l / D, a.r) < make_pair(b.l / D, b.r);
    }
} qs[MAX_N];
int cnt[1000001], a[MAX_N], odd;
bool ans[MAX_N];

void add(int idx) {
    if (idx <= 0) {
        return;
    }
    cnt[a[idx]]++;
    if (cnt[a[idx]] % 2) odd++;
    else odd--;
}
void remove(int idx) {
    if (idx <= 0) {
        return;
    }
    cnt[a[idx]]--;
    if (cnt[a[idx]] % 2) odd++;
    else odd--;
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int tt; cin >> tt;
    while (tt--) {
        int n, q;   cin >> n >> q;
        for (int i = 1; i <= n; i++) {
            cin >> a[i];
            cnt[a[i]] = 0;
        }
        for (int i = 1; i <= q; i++) {
            int l, r;   cin >> l >> r;
            qs[i] = query(l, r, i);
        }
        sort(qs + 1, qs + 1 + q);
        int curL = 1, curR = 1;
        odd = 1;
        cnt[a[1]]++;
        for (int i = 1; i <= q; i++) {
            int l = qs[i].l, r = qs[i].r;
            while (curL < l) remove(curL++);
            while (curL > l) add(--curL);
            while (curR > r) remove(curR--);
            while (curR < r) add(++curR);
            ans[qs[i].id] = odd;
        }
        for (int i = 1; i <= q; i++) {
            cout << (ans[i] ? "NO\n" : "YES\n");
        }
    }

    return 0;
}
```