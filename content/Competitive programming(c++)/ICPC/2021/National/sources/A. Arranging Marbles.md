```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;

const int MAX_N = 1e6 + 1;
ll cnt[10001], a[MAX_N];
set<pair<int, int>> st; // To store unique values and their frequencies
vector<pair<int, int>> color[1001]; // Stores the row assignment for each color

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n;
    cin >> n;

    // Step 1: Read input and count frequencies of each element in `a`
    for (int i = 1; i <= n * n; i++) {
        cin >> a[i];
        cnt[a[i]]++;
    }

    // Step 2: Populate the set `st` with unique elements and their frequencies
    for (int i = 1; i <= n * n; i++) {
        if (cnt[a[i]] > 0) {  // Only add each unique value once
            st.insert({cnt[a[i]], a[i]});
            cnt[a[i]] = 0;  // Reset to prevent duplicate insertions
        }
    }

    // Step 3: Distribute colors to each row based on frequencies
    for (int i = 1; i <= n; i++) {
        auto sl1 = *st.begin();         // Element with the smallest frequency
        auto sl2 = *prev(st.end());     // Element with the largest frequency

        // Case 1: If we need more colors than `sl1` provides
        if (n - sl1.first > 0) {
            // Remove these two elements from the set temporarily
            st.erase(st.begin());
            st.erase(prev(st.end()));

            // Update `sl2` by reducing its count by the difference
            st.insert({sl2.first - (n - sl1.first), sl2.second});
            
            // Assign `sl1` and `sl2` colors to row `i`
            color[sl1.second].push_back({i, sl1.first});
            color[sl2.second].push_back({i, n - sl1.first});
        } else {
            // Case 2: Assign entire row `i` to `sl1` and update its frequency
            st.erase(st.begin());
            color[sl1.second].push_back({i, n});
            if (sl1.first - n > 0) {
                st.insert({sl1.first - n, sl1.second});
            }
        }
    }

    // Step 4: Output the result
    cout << "YES\n";
    for (int i = 1; i <= n * n; i++) {
        int sz = color[a[i]].size() - 1;
        cout << color[a[i]][sz].first << ' ';
        
        // Decrease the count for the assigned row and remove it if it's fully used
        color[a[i]][sz].second--;
        if (color[a[i]][sz].second == 0) color[a[i]].pop_back();
    }

    return 0;
}

```