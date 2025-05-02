```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;

const ll MOD = 1e9 + 7;
const int N = 1e5 + 5;
string L, R;
double dp[N][2][2][2];
bool vis[N][2][2][2];
struct Trace {
    int ta, tb, end0, digit;
};
Trace trace[N][2][2][2];

bool umax(double &a, double b) {
    if (a < b) {
        a = b;
        return true;
    }
    return false;
}

double solve(int pos, int ta, int tb, int end0) {
    if (pos == (int)R.size()) return 0;

    double &res = dp[pos][ta][tb][end0];
    if (vis[pos][ta][tb][end0]) return res;

    vis[pos][ta][tb][end0] = true;
    res = -1e18;

    for (int i = (ta ? 0 : L[pos] - '0'); i <= (tb ? 9 : R[pos] - '0'); ++i) {
        int nta = (ta || (i > L[pos] - '0'));
        int ntb = (tb || (i < R[pos] - '0'));
        int nend0 = end0 || (i > 0);
        double ndp = solve(pos + 1, nta, ntb, nend0);

        if (nend0 && i > 0) ndp += log2(i);

        if (umax(res, ndp)) {
            trace[pos][ta][tb][end0] = {nta, ntb, nend0, i};
        }
    }
    return res;
}

void constructResult() {
    int i = 0, j = 0, k = 0, end0 = 0;
    ll ans = 1;

    while (i < (int)R.size()) {
        Trace temp = trace[i][j][k][end0];
        if (end0 || temp.digit > 0) ans = ans * temp.digit % MOD;
        i++;
        j = temp.ta;
        k = temp.tb;
        end0 = temp.end0;
    }
    cout << ans << '\n';
}

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(nullptr);

    cin >> L >> R;
    while (L.size() < R.size()) L = '0' + L;

    memset(dp, 0, sizeof dp);
    memset(vis, 0, sizeof vis);

    solve(0, 0, 0, 0);
    constructResult();
    return 0;
}

```