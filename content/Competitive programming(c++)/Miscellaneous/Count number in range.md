```cpp
const int MAX_N = 1e5 + 1;
vector<int> pos[MAX_N];
int a[MAX_N];
void init() {
	for (int i = 0; i < MAX_N; i++) {
		pos[i].push_back(0);
	}
	for (int i = 1; i <= MAX_N; i++) {
		pos[a[i]].push_back(i);
	}
}
int count_number_in_range(int l, int r, int val) {
	auto it1 = lower_bound(pos[val].begin(), pos[val].end(), l); it1--;
    auto it2 = upper_bound(pos[val].begin(), pos[val].end(), r); it2--;
	return it2 - it1;
}
```