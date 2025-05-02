```cpp
#include<bits/stdc++.h>
using namespace std;
 
#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif
 
const int MAX_N = 2e5 + 1;
int a[MAX_N];
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
	    // Remove all the element bigger or equal a[i]
        while (st.size() && a[st.top()] >= a[i]) {
            st.pop();
        }
        // If have element smaller than a[i] then print it
        // If not, it means a[i] is the biggest from the left
        if (st.size()) {
            cout << st.top() << ' ';
        }
        else {
            cout << 0 << ' ';
        }
        st.push(i);
    }
 
    return 0;
}

```