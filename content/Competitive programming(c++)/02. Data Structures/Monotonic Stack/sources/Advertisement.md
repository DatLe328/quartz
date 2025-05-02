The main idea is find the nearest smaller element to the left and right, if want to know how to find it go to this [[Nearest Smaller Values|problem]]
```cpp
#include<bits/stdc++.h>
using namespace std;
 
#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif
 
const int MAX_N = 2e5 + 1;
int64_t a[MAX_N], l[MAX_N], r[MAX_N];
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
        st.push(i);
    }
    while (st.size()) st.pop();
    for (int i = n; i > 0; i--) {
        while (st.size() && a[st.top()] >= a[i]) {
            st.pop();
        }
        if (st.size()) {
            r[i] = st.top();
        }
        st.push(i);
    }
    int64_t ans = 0;
    for (int i = 1; i <= n; i++) {
        l[i]++;
        if (r[i] == 0) {
            r[i] = n + 1;
        }
        ans = max(ans, (r[i] - l[i]) * a[i]);
    }
    cout << ans;
 
    return 0;
}

```