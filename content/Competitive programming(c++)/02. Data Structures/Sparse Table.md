```cpp
struct SparseTable {
    vector<vector<int>> table;
    const int MAX_L = 20;

    // arr's index start from 0
    // table's index start from 1
    SparseTable(const vector<int>& arr) {
        int n = (int)arr.size();
        table.resize(n + 1, vector<int>(MAX_L, 0));
        for (int i = 1; i <= n; i++) {
            table[i][0] = arr[i - 1]; 
        }
        for (int k = 1; k < MAX_L; k++) {
            for (int i = 1; i + (1 << k) - 1 <= n; i++) {
                table[i][k] = min(table[i][k - 1], table[i + (1 << (k - 1))][k - 1]);
                // table[i][k] = table[i][k - 1] ^ table[i + (1 << (k - 1))][k - 1];
            }
        }
    }

    int lg(int x) {
        return 31 - __builtin_clz(x);
    }

    int min_query(int l, int r) {
        int k = lg(r - l + 1); 
        return min(table[l][k], table[r - (1 << k) + 1][k]);
    }

    int xor_query(int l, int r) {
        int k = r - l + 1;
        int ret = 0;
        for (int i = 0; i < MAX_L; i++) {
            if (k & (1 << i)) {
                ret ^= table[l][i];
                l += (1 << i);
            }
        }
        return ret;
    }
};
```