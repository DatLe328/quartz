```cpp
#include <bits/stdc++.h>
using namespace std;

const double EPS = 1e-7;  // Sai số cho phép
const int ITERATIONS = 100;  // Số vòng lặp của tìm kiếm tam phân

// Hàm tính thể tích V(h) với chiều cao h
double volume(double h, double X, double Y) {
    return (X - 2 * h) * (Y - 2 * h) * h;
}

// Tìm kiếm tam phân để tìm giá trị h tối ưu
double ternary_search(double X, double Y) {
    double left = 0, right = min(X, Y) / 2.0;
    
    for (int i = 0; i < ITERATIONS; i++) {
        double h1 = (2 * left + right) / 3.0;
        double h2 = (left + 2 * right) / 3.0;
        
        if (volume(h1, X, Y) > volume(h2, X, Y)) {
            right = h2;
        } else {
            left = h1;
        }
    }
    
    return volume((left + right) / 2.0, X, Y);
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    int T;
    cin >> T;
    
    while (T--) {
        double X, Y;
        cin >> X >> Y;
        
        // Tìm thể tích lớn nhất và in kết quả
        cout << fixed << setprecision(9) << ternary_search(X, Y) << "\n";
    }
    
    return 0;
}

```