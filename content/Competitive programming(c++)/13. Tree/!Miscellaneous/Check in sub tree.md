```cpp
bool checkInSubtree(int u, int root) { 
	return num[root] <= num[u] && num[u] <= tail[root]; 
}
```