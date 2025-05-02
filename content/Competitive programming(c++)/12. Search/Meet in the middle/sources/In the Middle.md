```cpp
#include<bits/stdc++.h>
using namespace std;

int a[41];
int n, x;
vector<int> block_left, block_right;

void try_left(int idx, int sum) {
    if (sum > x) {
        return;
    }
    if (idx == n / 2) {
        block_left.push_back(sum);
        return;
    }
    try_left(idx + 1, sum);
    try_left(idx + 1, sum + a[idx]);
}

void try_right(int idx, int sum) {
    if (sum > x) {
        return;
    }
    if (idx == n) {
        block_right.push_back(sum);
        return;
    }
    try_right(idx + 1, sum);
    try_right(idx + 1, sum + a[idx]);
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    if(fopen("A_input.txt", "r")) {
        freopen("A_input.txt", "r", stdin);
    }

    cin >> n >> x;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    try_left(0, 0);
    try_right(n / 2, 0);
    sort(block_right.begin(), block_right.end());
    int64_t ans = 0;
    for (int i : block_left) {
        auto it1 = lower_bound(block_right.begin(), block_right.end(), x - i);
        auto it2 = upper_bound(block_right.begin(), block_right.end(), x - i);
        ans += (it2 - block_right.begin()) - (it1 - block_right.begin());
    }
    cout << ans;


    return 0;
}

```