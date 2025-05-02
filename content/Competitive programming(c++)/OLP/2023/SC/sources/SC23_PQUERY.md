```cpp
#include<bits/stdc++.h>
using namespace std;

constexpr int BLOCK_SIZE = 400;
constexpr int MAX_N = 150000 + 10;
constexpr int MAX_BLOCKS = MAX_N / BLOCK_SIZE + 5;

int64_t blockSum[MAX_BLOCKS];      // Tổng giá trị các phần tử trong mỗi khối
int prefix[MAX_BLOCKS][MAX_BLOCKS]; // Ma trận tiền tố cho các khối
int64_t lazyAdd[MAX_BLOCKS];        // Giá trị cần thêm vào mỗi khối (lazy propagation)
int64_t values[MAX_N];              // Mảng giá trị ban đầu
int indexMap[MAX_N], reverseMap[MAX_N]; // Ánh xạ chỉ số của mảng
int queryType[MAX_N], leftIdx[MAX_N], rightIdx[MAX_N], val[MAX_N]; // Thông tin truy vấn
int numElements, numQueries;
int prevQuery[MAX_N];
int64_t queryResults[MAX_N];
vector<int> queryTree[MAX_N];

inline void rangeUpdate(int left, int right, int increment) {
    int leftBlock = left / BLOCK_SIZE, rightBlock = right / BLOCK_SIZE;
    if (leftBlock == rightBlock) {
        for (int i = left; i <= right; i++) {
            values[indexMap[i]] += increment;
            blockSum[indexMap[i] / BLOCK_SIZE] += increment;
        }
    } else {
        for (int i = left; i < (leftBlock + 1) * BLOCK_SIZE && i < numElements; i++) {
            values[indexMap[i]] += increment;
            blockSum[indexMap[i] / BLOCK_SIZE] += increment;
        }
        for (int i = right; i >= rightBlock * BLOCK_SIZE; i--) {
            values[indexMap[i]] += increment;
            blockSum[indexMap[i] / BLOCK_SIZE] += increment;
        }
        for (int i = leftBlock + 1; i <= rightBlock - 1; i++) {
            lazyAdd[i] += increment;
        }
    }
}

inline void applyLazy(int block) {
    for (int j = block * BLOCK_SIZE; j < (block + 1) * BLOCK_SIZE && j < numElements; j++) {
        values[indexMap[j]] += lazyAdd[block];
        blockSum[indexMap[j] / BLOCK_SIZE] += lazyAdd[block];
    }
    lazyAdd[block] = 0;
}

inline void swapElements(int x, int y) {
    if (x == y) return;
    applyLazy(x / BLOCK_SIZE), applyLazy(y / BLOCK_SIZE);
    for (int i = indexMap[x] / BLOCK_SIZE; i * BLOCK_SIZE < numElements; i++) prefix[i][x / BLOCK_SIZE]--;
    for (int i = indexMap[y] / BLOCK_SIZE; i * BLOCK_SIZE < numElements; i++) prefix[i][y / BLOCK_SIZE]--;
    swap(indexMap[x], indexMap[y]);
    reverseMap[indexMap[x]] = x, reverseMap[indexMap[y]] = y;
    for (int i = indexMap[x] / BLOCK_SIZE; i * BLOCK_SIZE < numElements; i++) prefix[i][x / BLOCK_SIZE]++;
    for (int i = indexMap[y] / BLOCK_SIZE; i * BLOCK_SIZE < numElements; i++) prefix[i][y / BLOCK_SIZE]++;
}

inline int64_t getSum(int x) {
    if (x < 0) return 0;
    int blockX = x / BLOCK_SIZE;
    int64_t result = 0;
    for (int i = blockX * BLOCK_SIZE; i <= x; i++) {
        result += values[i] + lazyAdd[reverseMap[i] / BLOCK_SIZE];
    }
    blockX--;
    if (blockX < 0) return result;
    for (int i = 0; i * BLOCK_SIZE < numElements; i++) {
        result += lazyAdd[i] * prefix[blockX][i];
    }
    for (int i = 0; i <= blockX; i++) result += blockSum[i];
    return result;
}

void processQueryTree(int node) {
    if (queryType[node] == 1) {
        rangeUpdate(leftIdx[node], rightIdx[node], +val[node]);
    } else if (queryType[node] == 3) {
        swapElements(leftIdx[node], rightIdx[node]);
    } else if (queryType[node] == 2) {
        queryResults[node] = getSum(rightIdx[node]) - getSum(leftIdx[node] - 1);
    }

    for (int child : queryTree[node]) processQueryTree(child);

    if (queryType[node] == 1) {
        rangeUpdate(leftIdx[node], rightIdx[node], -val[node]);
    } else if (queryType[node] == 3) {
        swapElements(leftIdx[node], rightIdx[node]);
    }
}

void initializePrefix() {
    for (int i = 0; i * BLOCK_SIZE < numElements; i++) {
        for (int j = i * BLOCK_SIZE; j < (i + 1) * BLOCK_SIZE && j < numElements; j++) {
            prefix[i][reverseMap[j] / BLOCK_SIZE]++;
        }
        for (int j = 0; j * BLOCK_SIZE < numElements; j++) {
            prefix[i][j] += i ? prefix[i - 1][j] : 0;
        }
    }
}

int32_t main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cin >> numElements >> numQueries;
    for (int i = 0; i < numElements; i++) cin >> values[i], blockSum[i / BLOCK_SIZE] += values[i];
    for (int i = 0; i < numElements; i++) cin >> indexMap[i], indexMap[i]--;
    for (int i = 0; i < numElements; i++) reverseMap[indexMap[i]] = i;
    int lastQuery = 0;
    for (int i = 1; i <= numQueries; i++) {
        cin >> queryType[i];
        prevQuery[i] = lastQuery;
        if (queryType[i] == 1) {
            cin >> leftIdx[i] >> rightIdx[i] >> val[i];
            leftIdx[i]--, rightIdx[i]--;
        } else if (queryType[i] == 2) {
            cin >> leftIdx[i] >> rightIdx[i];
            leftIdx[i]--, rightIdx[i]--;
        } else if (queryType[i] == 3) {
            cin >> leftIdx[i] >> rightIdx[i];
            leftIdx[i]--, rightIdx[i]--;
        } else {
            int x;
            cin >> x;
            prevQuery[i] = x - 1;
        }
        queryTree[prevQuery[i]].emplace_back(i);
        lastQuery = i;
    }

    initializePrefix();
    processQueryTree(0);

    for (int i = 1; i <= numQueries; i++) {
        if (queryType[i] == 2) {
            cout << queryResults[i] << '\n';
        }
    }
}

```