# Thuật toán Moore

- Còn được gọi là **Majority Vote Algorithm**, Moore’s Algorithm xác định phần tử xuất hiện nhiều hơn một nửa trong một mảng (phần tử đa số).
    
- Thuật toán này hoạt động bằng cách duyệt qua mảng và sử dụng hai biến để theo dõi ứng cử viên hiện tại và số đếm của nó.
    

## Độ phức tạp

- **Thời gian**: O(n)
- **Không gian**: O(1)
```cpp
int boyer_moore_majority_vote_algorithm(const vector<int>& a) {
    // find the majority element among the given elements that have more than N/ 2 occurrences
    int n = a.size();
    int candidate = -1, votes = 0;
    for (int i = 0; i < n; i++) {
        if (votes == 0) {
            candidate = a[i];
            votes = 1;
        }
        else {
            if (a[i] == candidate) {
                votes++;
            }
            else {
                votes--;
            }
        }
    }
    int count = 0;
    for (int i = 0; i < n; i++) {
        if (a[i] == candidate) {
            count++;
        }
    }
    if (count > n / 2) {
        return candidate;
    }
    return -1;
}

  

vector<int> majorityElement(vector<int>& nums, int k)
{
    int n = nums.size();
    // Initialize an array of pairs to store k-1
    // candidates and their counts
    vector<pair<int, int> > candidates(k - 1);
    // Initialize candidate elements
    // and their counts to 0
    for (int i = 0; i < k - 1; i++) {
        candidates[i] = { 0, 0 };
    }
    // Step 1: Scanning the Array
    for (int num : nums) {
        bool found = false;
        for (int j = 0; j < k - 1; j++) {
            // If the element exists in
            // candidates
            if (candidates[j].first == num) {
                // Increment its count
                candidates[j].second++;
                found = true;
                break;
            }
        }
        // If the element is not in candidates
        if (!found) {
            for (int j = 0; j < k - 1; j++) {
                // Find an empty slot
                if (candidates[j].second == 0) {
                    // Insert as a new candidate
                    candidates[j] = { num, 1 };
                    found = true;
                    break;
                }
            }
        }
        // If all slots are occupied
        if (!found) {
            // Decrement counts
            // of all candidates
            for (int j = 0; j < k - 1; j++) {
                candidates[j].second--;
            }
        }
    }
    // Initialize a vector to store
    // the final results
    vector<int> ans;
    // Step 2: Verification
    for (int i = 0; i < k - 1; i++) {
        int count = 0;
        for (int j = 0; j < n; j++) {
            if (nums[j] == candidates[i].first) {
                count++;
            }
        }
        // If the count is greater than n/k
        if (count > n / k) {
            // Add the candidate
            // to the result
            ans.push_back(candidates[i].first);
        }
    }
    return ans;
}
```
