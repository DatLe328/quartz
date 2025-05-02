```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

#ifdef LOCAL
#include "debug.h"
#else
#define debug(...) 42
#endif

ll start = chrono::steady_clock().now().time_since_epoch().count();
mt19937_64 rnd(start);

inline int rand(const int& l, const int& r) {
    return uniform_int_distribution<int>(l, r)(rnd);
}

const int MAX_N = 251;
int n, m, k;
bool vis[MAX_N];
vector<int> idx;
int a[MAX_N];
vector<int> adj[MAX_N];

// Function to calculate region sum
ll calc_region(vector<vector<int>> region, int i) {
    ll ret = 0;
    for (auto j : region[i]) {
        ret += a[j];
    }
    return ret;
}

// Function to debug region assignments
void debug_region(vector<vector<int>> region) {
    for (int i = 0; i < k; i++) {
        cout << "Region " << i << ": {";
        for (int j : region[i]) {
            cout << j << ", ";
        }
        cout << "}, Region val: " << calc_region(region, i) << '\n';
    }
}

int diff = INT_MAX;
vector<vector<int>> ans;

// Solve function for random group assignments
void solve() {
    idx.resize(n);
    iota(idx.begin(), idx.end(), 1);
    vector<vector<int>> region(k);
    vector<int> region_val(k);
    memset(vis, false, sizeof vis);
    shuffle(idx.begin(), idx.end(), rnd);

    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq; // [region_val, region_id]
    for (int i = 0; i < k; i++) {
        int u = idx[i];
        vis[u] = 1;
        region[i].push_back(u);
        region_val[i] += a[u];
        pq.push({region_val[i], i});
    }

    while (pq.size()) {
        auto [w, id] = pq.top();
        pq.pop();

        int mx = -1, res = -1;
        for (int u : region[id]) {
            for (int v : adj[u]) {
                if (!vis[v] && a[v] > mx) {
                    mx = a[v];
                    res = v;
                }
            }
        }

        if (mx != -1) {
            vis[res] = 1;
            region[id].push_back(res);
            region_val[id] += mx;
            pq.push({region_val[id], id});
        }
    }

    int iter = 50;
    while (k > 1 && iter--) {
        int u = rand(0, k - 2);
        int v = rand(u + 1, k - 1);
        int state = abs(region_val[u] - region_val[v]);

        for (int node_i : region[u]) {
            for (int node_j : region[v]) {
                if (find(adj[node_i].begin(), adj[node_i].end(), node_j) == adj[node_i].end()) continue;
                swap(node_i, node_j);

                int newSumI = region_val[u] - a[node_i] + a[node_j];
                int newSumJ = region_val[v] - a[node_j] + a[node_i];
                int newDiff = abs(newSumI - newSumJ);

                if (newDiff < state) {
                    state = newDiff;
                } else {
                    swap(node_i, node_j); // Revert if no improvement
                }
            }
        }
    }

    int max_val = *max_element(region_val.begin(), region_val.end());
    int min_val = *min_element(region_val.begin(), region_val.end());

    if (max_val - min_val < diff) {
        diff = max_val - min_val;
        ans = region;
    }
}

void solve_sub1() {
    vector<pair<int, int>> subtree_sum(n, {0, 0});
    
    function<void(int, int)> dfs = [&] (int u, int p) {
        subtree_sum[u - 1] = {a[u], u}; 
        for (int v : adj[u]) {
            if (v == p) continue;
            dfs(v, u);  
            subtree_sum[u - 1].first += subtree_sum[v - 1].first; 
        }
    };
    dfs(1, -1);  
    sort(subtree_sum.begin(), subtree_sum.end()); 
    
    vector<vector<int>> dp(n + 1, vector<int>(k + 1, INT_MAX));  
    dp[0][0] = 0;  
    
    // Tính toán DP
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= k; ++j) {
            for (int m = 0; m < i; ++m) {
                int diff = abs(subtree_sum[i - 1].first - subtree_sum[m].first); // Chênh lệch giữa các nhóm
                dp[i][j] = min(dp[i][j], dp[m][j - 1] + diff); // Lựa chọn phân chia tối ưu
            }
        }
    }

    // Truy vết lại các nhóm
    vector<int> selected_groups;
    vector<int> groupAssign(n, -1);
    int i = n, j = k;
    while (i > 0 && j > 0) {
        for (int m = 0; m < i; ++m) {
            int diff = subtree_sum[i - 1].first - subtree_sum[m].first;
            if (dp[i][j] == dp[m][j - 1] + diff) { 
                selected_groups.push_back(m + 1); 
                i = m;
                j--;
                break;
            }
        }
    }

    reverse(selected_groups.begin(), selected_groups.end());

    int groupIndex = 0;
    for (int idx : selected_groups) {
        for (int i = idx; i <= n; ++i) {
            if (groupAssign[i - 1] == -1) {  
                groupAssign[i - 1] = groupIndex;
            }
        }
        groupIndex++;
    }

    // In các nhóm và thành viên của từng nhóm
    for (int group = 0; group < k; ++group) {
        cout << "Group " << group + 1 << ": ";
        for (int i = 0; i < n; ++i) {
            if (groupAssign[i] == group) {
                cout << i + 1 << " ";  // In ra đỉnh của nhóm
            }
        }
        cout << endl;
    }
}


int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    cin >> n >> m >> k;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    if (k <= 50 && m == n - 1) {
        solve_sub1();
        return 0;
    } else {
        while (true) {
            ll cur = chrono::steady_clock().now().time_since_epoch().count();
            if (cur - start > 0.98 * 1e9) break;
            solve();
        }
    }

    for (auto i : ans) {
        cout << i.size() << '\n';
        for (int j : i) {
            cout << j << ' ';
        }
        cout << '\n';
    }

    return 0;
}

```