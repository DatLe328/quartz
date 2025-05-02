
```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

ll MOD;
ll Mul(ll a, ll b, ll m) {
	if (b == 0) {
		return 0;
	}
	if (b == 1) {
		return a;
	}
	ll t = Mul(a, b / 2, m);
	if (b & 1) {
		return ((t + t) % m + a % m) % m;
	}
	else {
		return (t + t) % m;
	}
}

struct Matrix {
	ll mat[2][2];
	Matrix friend operator* (Matrix a, Matrix b) {
		Matrix ret;
		for (int i = 0; i < 2; i++) {
			for (int j = 0; j < 2; j++) {
				ret.mat[i][j] = 0;
				for (int k = 0; k < 2; k++) {
					ret.mat[i][j] = (ret.mat[i][j] + Mul(a.mat[i][k], b.mat[k][j], MOD)) % MOD;
				}
			}
		}
		return ret;
	}
};

Matrix Pow(Matrix base, ll n) {
	Matrix ret {{
		{1, 0},
		{0, 1}}};
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

	ll a, b;	cin >> a >> b >> MOD;
	Matrix base {{
		{1, 1},
		{1, 0}}};
	cout << Pow(base, __gcd(a, b)).mat[0][1];

	return 0;
}

```