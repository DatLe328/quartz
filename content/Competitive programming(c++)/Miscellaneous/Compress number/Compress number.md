# Method 1: Use map
Use for solving problem are not depending on original value like distinct value
https://cses.fi/problemset/task/1734/
```cpp
void compress() {
    map<int, int> mp;
    for (int i = 1; i <= n; i++) {
        mp[a[i]] = i;
    }
    for (int i = 1; i <= n; i++) {
        a[i] = mp[a[i]];
    }
}
```

# Method 2: Hashing
With this method we can trace back original value
[[CT23_ATTINDEX]]
```cpp
vector<int> d = a;
sort(d.begin(), d.end());
d.resize(unique(d.begin(), d.end()) - d.begin());
for (int i = 0; i < n; ++i) {
  a[i] = lower_bound(d.begin(), d.end(), a[i]) - d.begin();
}
//original value of a[i] can be obtained through d[a[i]]
```