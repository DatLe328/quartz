
BFS 0-1 là phiên bản của BFS được sử dụng cho đồ thị có cạnh trọng số 0 hoặc 1.

Sử dụng deque để đẩy đỉnh với trọng số 0 vào đầu và đỉnh với trọng số 1 vào cuối, từ đó tối ưu hóa việc duyệt các cạnh trọng số 0 và 1.

## Độ phức tạp

- **Thời gian**: O(V + E)
- **Không gian**: O(V)

```cpp
const int INF = 1e9;
const int MAXN = 2e6+5;

int n; // Số đỉnh của đồ thị
int D[MAXN], vis[MAXN];
vector<pair<int, int>> g[MAXN];  // first là đầu mút của cạnh, second là trọng số của cạnh
deque<int> dq;

void BFS_01(int s){
    for(int i = 1; i <= n; i++){
        D[i] = INF;              // Khởi tạo
        vis[i] = 0;
    }
    D[s] = 0;
    dq.push_front(s);
    while(!dq.empty()){
        int u = dq.front();
        dq.pop_front();
        if(vis[u])continue;
        vis[u] = 1;             // Đánh dấu
        for(auto v: g[u]){
            if(D[v.first] > D[u] + v.second){              //
                D[v.first] = D[u] + v.second;              // Duyệt và sử lý các cạnh của u
                if(v.second == 1)dq.push_back(v.first);    //
                else dq.push_front(v.first);
            }
        }
    }
}
```

```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0);

	int tt;	cin >> tt;
	while (tt--) {
		int n, m;	cin >> n >> m;
		vector<string> a(n);
		for (auto& i : a) {
			cin >> i;
		}
		int dx[] {0, -1, 1, 0};
		int dy[] {-1, 0, 0, 1};
		deque<pair<int, int>> dq;
		dq.push_back({0, 0});
		vector<vector<int>> dist(n, vector<int>(m, INT_MAX));
		vector<vector<int>> vis(n, vector<int>(m, 0));
		dist[0][0] = 0;
		while (dq.size()) {
			auto [i, j] = dq.front();
			dq.pop_front();
			if (vis[i][j]) continue;
			vis[i][j] = 1;
			for (int k = 0; k < 4; k++) {
				int u = i + dy[k];
				int v = j + dx[k];
				if (u < 0 || v < 0 || u == n || v == m) {
					continue;
				}
				int cost = 0;
				if (a[i][j] != a[u][v]) {
					cost = 1;
				}
				if (dist[i][j] + cost < dist[u][v]) {
					dist[u][v] = dist[i][j] + cost;
					if (cost == 1) {
						dq.push_back({u, v});
					}
					else {
						dq.push_front({u, v});
					}
				}
			}
		}
		cout << dist[n - 1][m - 1] << '\n';
	}

	return 0;
}

```