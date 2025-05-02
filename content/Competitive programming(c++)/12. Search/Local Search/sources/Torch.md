```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif


mt19937_64 rng(chrono::steady_clock::now().time_since_epoch().count());
int rand_int(int l, int r) {
    return uniform_int_distribution<int>(l, r)(rng);
}
double rand_double(double l, double r) {
    return uniform_real_distribution<double>(l, r)(rng);
}

const int MAX_N = 100;
const double MAX_TIME = 0.87;
double dist[MAX_N][MAX_N];
int perm[MAX_N], best_perm[MAX_N];
int x[MAX_N], y[MAX_N];
int n;
double start_temp = 1000.0;
const double end_temp = 0.01;
double Ratio = end_temp / start_temp;

void solve() {
    iota(perm, perm + n, 0);
    shuffle(perm + 1, perm + n, rng);

    double prev_score = dist[perm[n - 1]][perm[0]];
    for (int i = 0; i < n - 1; i++) {
        prev_score += dist[perm[i]][perm[i + 1]];
    }
    double best_score = prev_score;
    copy(perm, perm + n, best_perm);

    double elapsed_time = 1.0 * clock() / CLOCKS_PER_SEC;
    double elapsed_frac = elapsed_time / MAX_TIME;
    double temp = start_temp * pow(Ratio, elapsed_frac);
    int iter = 0;
    while (true) {
        iter++;
        if ((iter & 150) == 0) {
            elapsed_time = 1.0 * clock() / CLOCKS_PER_SEC;
            elapsed_frac = elapsed_time / MAX_TIME;
            temp = start_temp * pow(Ratio, elapsed_frac);
        }
        if (elapsed_time > MAX_TIME) break;
        int l = rand_int(1, n - 2), r = rand_int(l + 2, n);
        int v1 = perm[l - 1], v2 = perm[l];
        int v3 = perm[r - 1], v4 = (r < n) ? perm[r] : perm[0];
        double score_delta = dist[v1][v3] + dist[v2][v4] - dist[v1][v2] - dist[v3][v4];
        double new_score = prev_score + score_delta;
        if (score_delta < 0 || exp(-score_delta / temp) >= rand_double(0.0, 1.0)) {
            prev_score = new_score;
            reverse(perm + l, perm + r);
        }
        if (new_score < best_score) {
            best_score = new_score;
            copy(perm, perm + n, best_perm);
        }
    }
    cout << setprecision(3) << fixed << best_score << '\n';
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> x[i] >> y[i];
    }
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            dist[i][j] = dist[j][i] = hypot(x[i] - x[j], y[i] - y[j]);
        }
    }
    solve();
    for (int i = 0; i < n; i++) {
        cout << best_perm[i] + 1 << ' ';
    }

    return 0;
}
```