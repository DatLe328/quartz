# Solution 1: Hill Climbing
```cpp
#include <bits/stdc++.h>
using namespace std;

struct Point {
    double x, y;
    Point() : x(0), y(0) {}
    Point(double x, double y) : x(x), y(y) {}

    Point operator+(const Point &other) const {
        return Point(x + other.x, y + other.y);
    }

    Point rotate(double angle) const {
        return Point(x * cos(angle) - y * sin(angle), x * sin(angle) + y * cos(angle));
    }

    double distance(const Point &other) const {
        return sqrt((x - other.x) * (x - other.x) + (y - other.y) * (y - other.y));
    }
};

const double PI = acos(-1);
const int N_ITERATION = 10000;  // Số lần lặp
const double RATE = 0.99;  // Giảm chiều dài di chuyển sau mỗi lần lặp
const int NUM_DIRECTIONS = 100;  // Số hướng di chuyển

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0);

    Point A, B, C;
    cin >> A.x >> A.y >> B.x >> B.y >> C.x >> C.y;

    Point P = Point((A.x + B.x + C.x) / 3, (A.y + B.y + C.y) / 3);  // Bắt đầu từ trung điểm tam giác
    double len = 2000;  // Độ dài PP'

    for (int turn = 0; turn < N_ITERATION; turn++) {
        Point best = P;
        double bestDist = P.distance(A) + P.distance(B) + P.distance(C);

        // Tìm hướng tốt nhất từ P
        for (double angle = 0; angle < 2 * PI; angle += (2 * PI) / NUM_DIRECTIONS) {
            Point dir = Point(0, len).rotate(angle);  // Tạo vector với góc khác nhau
            Point Q = P + dir;  // Điểm Q = P'

            double currentDist = Q.distance(A) + Q.distance(B) + Q.distance(C);
            if (currentDist < bestDist) {
                best = Q;
                bestDist = currentDist;
            }
        }

        P = best;  // Cập nhật điểm P
        len *= RATE;  // Giảm độ dài PP' sau mỗi lần lặp
    }

    cout << fixed << setprecision(9) << P.x << " " << P.y << '\n';

    return 0;
}

```
# Solution 2: Ternary Search
```cpp
#include <bits/stdc++.h>
using namespace std;

const double INF = 1e9;
const double EPS = 1e-9;
const int ITERATIONS = 100;

struct Point {
    double x, y;
};

// Toa độ của 3 trung tâm mua sắm
Point shops[3];

// Trả lại tổng khoảng cách PA + PB + PC với P = (x, y)
double f(double x, double y) {
    double res = 0;
    for (int i = 0; i < 3; i++) {
        double dx = x - shops[i].x;
        double dy = y - shops[i].y;
        res += sqrt(dx * dx + dy * dy);
    }
    return res;
}

// Tìm kiếm tam phân theo trục y, khi đã cố định x
pair<double, double> ternary_search_y(double x) {
    double l = 0, r = 1000;
    for (int turn = 0; turn < ITERATIONS; turn++) {  // chặt 100 lần → độ chính xác là (⅔)^100 
        double y1 = l + (r - l) / 3.0;
        double y2 = r - (r - l) / 3.0;

        if (f(x, y1) < f(x, y2)) {
            r = y2;
        } else {
            l = y1;
        }
    }
    return {f(x, (l + r) / 2), (l + r) / 2};
}

// Tìm kiếm tam phân theo trục x
pair<double, double> ternary_search_x() {
    double l = 0, r = 1000;
    for (int turn = 0; turn < ITERATIONS; turn++) {  // chặt 100 lần → độ chính xác là (⅔)^100 
        double x1 = (2 * l + r) / 3.0;
        double x2 = (l + 2 * r) / 3.0;

        if (ternary_search_y(x1) < ternary_search_y(x2)) {
            r = x2;
        } else {
            l = x1;
        }
    }
    return {(l + r) / 2, ternary_search_y((l + r) / 2).second};
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);

    for (int i = 0; i < 3; i++) {
        cin >> shops[i].x >> shops[i].y;
    }

    pair<double, double> result = ternary_search_x();
    
    cout << fixed << setprecision(9) << result.first << " " << result.second << "\n";

    return 0;
}

```