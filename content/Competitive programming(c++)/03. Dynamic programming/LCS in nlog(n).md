## **Chỉ có thể sử dụng khi hai dãy là hoán vị của nhau**
```cpp
//https://codeforces.com/gym/102951/problem/C
#include<bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    if(fopen("A_input.txt", "r")) {
        freopen("A_input.txt", "r", stdin);
    }

    int n;  cin >> n;
    vector<int> a(n + 1), b(n + 1), mp(n + 1), f(n + 1);
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        mp[a[i]] = i;
    }
    for (int i = 1; i <= n; i++) { 
        cin >> b[i];
        f[i] = INT_MAX;
    }
    int len = 0;
    f[0] = 0;
    for (int i = 1; i <= n; i++) {
        int l = 0, r = len;
        if (mp[b[i]] > f[len]) {
            f[++len] = mp[b[i]];
        }
        else {
            while (l < r) {
                int mid = (l + r) / 2;
                if (f[mid] > mp[b[i]]) {
                    r = mid;
                }
                else {
                    l = mid + 1;
                }
            }
            f[l] = min(mp[b[i]], f[l]);
        }
    }
    cout << len;
    

    return 0;
}
```