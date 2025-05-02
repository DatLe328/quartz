#meet-in-the-middle
# Meet in the middle

Meet in the Middle là kỹ thuật chia bài toán thành hai nửa và giải quyết từng phần độc lập, sau đó kết hợp kết quả.

## Độ phức tạp

- **Thời gian**: O(2^(n/2)) cho các bài toán có kích thước n
- **Không gian**: O(2^(n/2)) cho lưu trữ các kết quả của nửa đầu tiên (hoặc cả hai nửa)

## Ứng dụng

- Các bài toán liên quan đến tìm kiếm và tối ưu hóa, như subset sum, knapsack problem.
- Tối ưu hóa các bài toán yêu cầu kiểm tra tất cả các khả năng trong không gian tìm kiếm lớn.
https://codeforces.com/contest/1006/problem/F
https://codeforces.com/contest/888/problem/E


```cpp
// https://cses.fi/problemset/task/1628/
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
https://codeforces.com/contest/888/problem/E
```cpp
#include<bits/stdc++.h>
using namespace std;

int64_t n, m, arr[35];
vector<int64_t> a, b;

void try_x(int idx, int64_t sum) {
    if (idx == n / 2) {
        a.push_back(sum % m);
        return;
    }
    try_x(idx + 1, sum);
    try_x(idx + 1, sum + arr[idx]);
}

void try_y(int idx, int64_t sum) {
    if (idx == n) {
        b.push_back(sum % m);
        return;
    }
    try_y(idx + 1, sum);
    try_y(idx + 1, sum + arr[idx]);
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    if(fopen("A_input.txt", "r")) {
        freopen("A_input.txt", "r", stdin);
    }
    cin >> n >> m;
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
    }
    try_x(0, 0);
    try_y(n / 2, 0);
    sort(b.begin(), b.end(), greater<int>());
    int64_t max_val = 0;
    for (auto i : a) {
        auto it = upper_bound(b.begin(), b.end(), m - i, greater<int>());
        max_val = max(max_val, (*it + i) % m);
    }
    cout << max_val;


    return 0;
}

```

https://codeforces.com/contest/1006/problem/F
```cpp
#include<bits/stdc++.h>
using namespace std;

int n, m, half;
int64_t ans = 0;
int64_t k, arr[21][21];
map<int64_t, int> mp[21][21];


void try_x(int i, int j, int cnt, int64_t xr) {
    xr ^= arr[i][j];
    if (cnt == half) {
        mp[i][j][xr]++;
        return;
    }
    if (i < n) {
        try_x(i + 1, j, cnt + 1, xr);
    }
    if (j < m) {
        try_x(i, j + 1, cnt + 1, xr);
    }
}

void try_y(int i, int j, int cnt, int64_t xr) {
    if (cnt == n + m - 2 - half) {
        if (mp[i][j].count(xr ^ k)) {
            ans += mp[i][j][xr ^ k];
        }
        return;
    }
    xr ^= arr[i][j];
    if (i > 1) {
        try_y(i - 1, j, cnt + 1, xr);
    }
    if (j > 1) {
        try_y(i, j - 1, cnt + 1, xr);
    }
}
int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    if(fopen("A_input.txt", "r")) {
        freopen("A_input.txt", "r", stdin);
    }

    cin >> n >> m >> k;
    half = (n + m - 2) / 2;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            cin >> arr[i][j];
        }
    }
    try_x(1, 1, 0, 0);
    try_y(n, m, 0, 0);

    cout << ans;
    return 0;
}
```