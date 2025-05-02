```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const ll MOD = 111539786;

struct Matrix {
    ll mat[2][2];
    Matrix() {
        mat[0][0] = mat[0][1] = mat[1][0] = mat[1][1] = 0;
    }
    Matrix friend operator * (const Matrix a, const Matrix b) {
        Matrix ret;
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 2; j++) {
                ret.mat[i][j] = 0;
                for (int k = 0; k < 2; k++) {
                    ret.mat[i][j] = (ret.mat[i][j] + a.mat[i][k] * b.mat[k][j]) % MOD;
                }
            }
        }
        return ret;
    }
};

Matrix Pow(Matrix base, ll n) {
    Matrix ret;
    ret.mat[0][0] = ret.mat[1][1] = 1;
    ret.mat[0][1] = ret.mat[1][0] = 0;
    
    while (n) {
        if (n & 1) {
            ret = ret * base;
        }
        base = base * base;
        n /= 2;
    }
    return ret;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    Matrix base, v, ans;
    base.mat[0][0] = 0;
    base.mat[0][1] = 1;
    base.mat[1][0] = 1;
    base.mat[1][1] = 1;
    
    v.mat[0][0] = 1;
    v.mat[1][0] = 1;
    
    int tt;    
    cin >> tt;
    while (tt--) {
        ll n;
        cin >> n;
        ans = Pow(base, n) * v;
        cout << ans.mat[0][0] << '\n';
    }

    return 0;
}

```