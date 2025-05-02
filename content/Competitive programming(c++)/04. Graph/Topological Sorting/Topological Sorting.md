# Sắp xếp topo (Topological Sort)

**Sắp xếp topo** là quá trình sắp xếp các đỉnh của đồ thị có hướng mà không có chu trình (DAG) sao cho đối với mọi cạnh u→v, đỉnh u đứng trước đỉnh v.

## Độ phức tạp

- **Thời gian**: O(V + E)
- **Không gian**: O(V)

## Ứng dụng

- **Lập kế hoạch công việc**: Xác định thứ tự thực hiện công việc có phụ thuộc lẫn nhau.
- **Phân tích phụ thuộc**: Trong ngôn ngữ lập trình và biên dịch.

```cpp
// https://cses.fi/problemset/task/1679
int n; // number of vertices
vector<vector<int>> adj; // adjacency list of graph
vector<bool> visited;
vector<int> ans;

void dfs(int v) {
    visited[v] = true;
    for (int u : adj[v]) {
        if (!visited[u])
            dfs(u);
    }
    ans.push_back(v);
}

void topological_sort() {
    visited.assign(n, false);
    ans.clear();
    for (int i = 0; i < n; ++i) {
        if (!visited[i]) {
            dfs(i);
        }
    }
    reverse(ans.begin(), ans.end());
}
```