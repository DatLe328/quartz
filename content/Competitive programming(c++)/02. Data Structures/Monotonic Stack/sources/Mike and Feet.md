```cpp
#include<bits/stdc++.h>
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

const int MAX_N = 2e5 + 1;
int a[MAX_N], l[MAX_N], r[MAX_N], ans[MAX_N];
int n;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    stack<int> st;
    for (int i = 1; i <= n; i++) {
        while (st.size() && a[st.top()] >= a[i]) {
            st.pop();
        }
        if (st.size()) {
            l[i] = st.top();
        }
        else {
            l[i] = 0;
        }
        st.push(i);
    }
    while (st.size())   st.pop();
    for (int i = n; i > 0; i--) {
        while (st.size() && a[st.top()] >= a[i]) {
            st.pop();
        }
        if (st.size()) {
            r[i] = st.top();
        }
        else {
            r[i] = n + 1;
        }
        st.push(i);
    }
    // a[i] is the smallest from [l[i] + 1, r[i] - 1]
    for (int i = 1; i <= n; i++) {
        int len = r[i] - l[i] - 1;
        ans[len] = max(ans[len], a[i]);
    }
    for (int i = n; i > 0; i--) {
        ans[i] = max(ans[i], ans[i + 1]);
    }
    for (int i = 1; i <= n; i++) {
        cout << ans[i] << ' ';
    }
    return 0;
}
```