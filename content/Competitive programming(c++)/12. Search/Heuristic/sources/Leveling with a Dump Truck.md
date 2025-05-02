```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int INF = INT_MAX;
int n;
pair<char, pair<int, int>> next_step_upload(int x, int y) {
    if (x % 2 == 0) {
        if (y == n - 1) return {'D', {x + 1, n - 1}};
        else            return {'R', {x, y + 1}};
    }

    if (x & 1) {
        if (y == 0)     return {'D', {x + 1, 0}};
        else            return {'L', {x, y - 1}};
    }
}

pair<char, pair<int, int>> next_step_download(int x, int y) {
    if (x % 2 == 0) {
        if (y == n - 1) return {'U', {x - 1, 0}};
        else            return {'L', {x, y - 1}};
    }

    if (x & 1) {
        if (y == 0)     return {'U', {x - 1, n - 1}};
        else            return {'R', {x, y + 1}};
    }
}
int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);

    cin >> n;
    vector<vector<int>> grid(n, vector<int>(n));
    for (auto &i : grid) {
        for (auto &j : i) {
            cin >> j;
        }
    }
    int soil_quantity = 0;
    pair<int, int> cur_pos = {0, 0};
    vector<pair<int, int>> remain;
    for (int i = 0; i < n * n; i++) {
        auto [x, y] = cur_pos;
        if (grid[x][y] > 0) {
            soil_quantity += grid[x][y];
            cout << "+" << grid[x][y] << '\n';
        }
        if (grid[x][y] < 0 && soil_quantity == abs(grid[x][y])) {
            soil_quantity += grid[x][y];
            cout << grid[x][y] << '\n';
            grid[x][y] = INF;
        }
        else if (grid[x][y] < 0 && soil_quantity < abs(grid[x][y])) {
            remain.push_back({x, y});
        }
        if (i == n * n - 1) continue;
        auto tmp = next_step_upload(x, y);
        cout << tmp.first << '\n';
        cur_pos = tmp.second;
    }
    sort(remain.begin(), remain.end());
    int sz = (int)remain.size();

    for (auto i = sz - 1; i >= 0; i--) {
        auto [x1, y1] = cur_pos;
        auto [x2, y2] = remain[i];
        
        int left = y2 - y1;
        int up = x1 - x2;

        for (int j = 0; j < up; j++) {
            cout << "U" << '\n';
        }
        if (left >= 0) {
            for (int j = 0; j < left; j++) {
                cout << "R" << '\n';
            }
        }
        else {
            left = abs(left);
            for (int j = 0; j < left; j++) {
                cout << "L" << '\n';
            }
        }
        cout << grid[x2][y2] << '\n';
        cur_pos = {x2, y2};
    }

    return 0;
}
```