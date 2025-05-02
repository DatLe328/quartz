	https://codeforces.com/problemset/problem/940/F
```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 1e5 + 1;
int block, block_time;

struct Query {
    int l, r;
    int time_update, time_query;

    bool operator< (Query other) {
        if (time_update / block_time != other.time_update / block_time) {
            return time_update / block_time < other.time_update / block_time;
        }
        if (l / block != other.l / block) {
            return l > other.l;
        }
        if (l / block & 1) {
            return r < other.r;
        }
        return r > other.r;
    }
} Q[MAX_N];

struct Update {
    int pos, val;
} T[MAX_N];

int freq[MAX_N], cnt[MAX_N * 2], sorted[MAX_N * 2], a[MAX_N], ans[MAX_N];
int n, q, n_q = 0, n_t = 0, sz = 0;

void add(int v) {
    freq[cnt[v]]--;
    cnt[v]++;
    freq[cnt[v]]++;
}

void remove(int v) {
    freq[cnt[v]]--;
    cnt[v]--;
    freq[cnt[v]]++;
}

void update(int id, int t) {
    if (Q[id].l <= T[t].pos && T[t].pos <= Q[id].r) {
        remove(a[T[t].pos]);
        add(T[t].val);
    }
    swap(a[T[t].pos], T[t].val);
}

int get_mex() {
    int p = 1;
    while (freq[p]) p++;
    return p;
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> q;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        sorted[++sz] = a[i];
    }   
    block = pow(n, 2.0 / 3);
    for (int i = 1; i <= q; i++) {
        int t;  cin >> t;
        if (t == 1) {
            int l, r;   cin >> l >> r;
            Q[++n_q] = {l, r, n_t, n_q};
        }
        else {
            int pos, val;   cin >> pos >> val;
            T[++n_t] = {pos, val};
            sorted[++sz] = val;
        }
    }
    block_time = pow(n_t, 2.0 / 3);
    sort(Q + 1, Q + 1 + n_q);
    sort(sorted + 1, sorted + 1 + sz);

    sz = unique(sorted + 1, sorted + 1 + sz) - sorted - 1;
    for (int i = 1; i <= n; i++) {
        a[i] = lower_bound(sorted + 1, sorted + 1 + sz, a[i]) - sorted;
    }

    for (int i = 1; i <= n_t; i++) {
        T[i].val = lower_bound(sorted + 1, sorted + 1 + sz, T[i].val) - sorted;
    }
    int timer = 0, cur_l = 1, cur_r = 0;
    for (int i = 1; i <= n_q; i++) {
        auto [l, r, time_update, time_query] = Q[i];
        while (cur_l < l) remove(a[cur_l++]);
        while (cur_l > l) add(a[--cur_l]);
        while (cur_r > r) remove(a[cur_r--]);
        while (cur_r < r) add(a[++cur_r]);

        while (timer < time_update) update(i, ++timer);
        while(timer > time_update) update(i, timer--);
        ans[time_query] = get_mex();
    }
    for (int i = 1; i <= n_q; i++) cout << ans[i] << '\n';

    return 0;
}
```