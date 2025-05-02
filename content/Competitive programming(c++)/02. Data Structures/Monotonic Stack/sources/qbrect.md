```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n, m;   cin >> n >> m;
    vector<vector<int>> a(n + 1, vector<int>(m + 1));
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            cin >> a[i][j];
        }
    }
    vector<vector<int>> sum(n + 1, vector<int>(m + 1));
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        vector<int> left(m + 1), right(m + 1);
        for (int j = 1; j <= m; j++) {
            if (a[i][j]) {
                sum[i][j] = sum[i - 1][j] + a[i][j];
            }
        }
        stack<int> st;
        for (int j = 1; j <= m; j++) {
            while (st.size() && sum[i][st.top()] >= sum[i][j]) {
                st.pop();
            }
            if (st.empty()) {
                left[j] = 0;
            }
            else {
                left[j] = st.top();
            }
            st.push(j);
        }
        while (st.size()) st.pop();
        for (int j = m; j > 0; j--) {
            while (st.size() && sum[i][st.top()] >= sum[i][j]) {
                st.pop();
            }
            if (st.empty()) {
                right[j] = m + 1;
            }
            else {
                right[j] = st.top();
            }
            st.push(j);
        }
        for (int j = 1; j <= m; j++) {
            ans = max(ans, sum[i][j] * (right[j] - left[j] - 1));
        }
    }
    cout << ans;

    return 0;
}
```