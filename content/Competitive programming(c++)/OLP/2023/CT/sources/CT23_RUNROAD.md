```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;

const int MAX_N = 2e5 + 1;
const ll mod = 1e9 + 7;
int n, t;
int h[MAX_N];
vector<int> adj[MAX_N];
ll ans = 0;
bool visited[MAX_N];

ll dfs1(int u, int par, const int &max_height)
{
	int ret = 1;
	visited[u] = true;
	for (int i = 0; i < (int)adj[u].size(); i++)
	{
		int v = adj[u][i];
		if (v == par || h[v] > max_height)
		{
			continue;
		}
		ret += dfs1(v, u, max_height);
	}
	return ret;
}

ll dfs2(int u, int par, const int &num_node, ll &total, const int &max_height)
{
	int ret = 1;
	for (int i = 0; i < (int)adj[u].size(); i++)
	{
		int v = adj[u][i];
		if (v == par || h[v] > max_height)
		{
			continue;
		}
		int num_child = dfs2(v, u, num_node, total, max_height);
		ret += num_child;
		ll mul = 1LL;
		(mul *= num_child) %= mod;
		(mul *= num_node - num_child) %= mod;
		(mul *= num_node - 2) %= mod;
		(total += mul) %= mod;
	}
	return ret;
}

ll solve(const int &max_height)
{
	for (int i = 1; i <= n; i++)
	{
		visited[i] = false;
	}
	ll ret = 0LL;
	for (int i = 1; i <= n; i++)
	{
		if (visited[i] == false && h[i] <= max_height)
		{
			int num_child = dfs1(i, -1, max_height);
			ll total = 0;
			dfs2(i, -1, num_child, total, max_height);
			(ret += total) %= mod;
		}
	}
	return ret;
}

int main()
{
	cin >> n >> t;
	for (int i = 1; i <= n; i++)
	{
		cin >> h[i];
	}
	for (int i = 1; i < n; i++)
	{
		int u, v;
		cin >> u >> v;
		adj[u].push_back(v);
		adj[v].push_back(u);
	}
	ans = solve(t);
	ans -= solve(t - 1);
	(ans += mod) %= mod;
	cout << ans;
	return 0;
}

```