```cpp
#include <bits/stdc++.h>
using namespace std;

struct Request {
    int start, end, duration;
};

bool compare(Request a, Request b) {
    return a.end < b.end;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    int n;
    cin >> n;
    vector<Request> requests(n);
    
    // Nhập các yêu cầu
    for (int i = 0; i < n; i++) {
        int start, end;
        cin >> start >> end;
        requests[i] = {start, end, end - start};
    }

    // Sắp xếp các yêu cầu theo thời gian kết thúc
    sort(requests.begin(), requests.end(), compare);

    // Mảng DP, dp[i] lưu tổng thời gian tối đa đến yêu cầu i
    vector<int> dp(n, 0);
    dp[0] = requests[0].duration;

    for (int i = 1; i < n; i++) {
        // Tính thời gian sử dụng nếu chọn yêu cầu i
        int include = requests[i].duration;

        // Tìm yêu cầu gần nhất trước đó không xung đột với yêu cầu i
        int j = -1;
        for (int k = i - 1; k >= 0; k--) {
            if (requests[k].end <= requests[i].start) {
                j = k;
                break;
            }
        }
        if (j != -1) {
            include += dp[j];
        }

        // Chọn giữa việc chọn hoặc bỏ qua yêu cầu i
        dp[i] = max(dp[i - 1], include);
    }

    // Kết quả là giá trị lớn nhất trong dp
    cout << dp[n - 1] << endl;
    return 0;
}

```