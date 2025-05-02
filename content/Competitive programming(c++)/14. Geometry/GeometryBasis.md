```cpp
template<typename T>
struct Point {
    typedef Point Vector;
    T x, y;
    Point() : x(0), y(0) {}
    Point(T x, T y) : x(x), y(y) {}

    Point operator+(const Point& b) const {
        return Point(x + b.x, y + b.y);
    }
    Point operator-(const Point& b) const {
        return Point(x - b.x, y - b.y);
    }
    Point operator*(T d) const {
        return Point(x * d, y * d);
    }
    Point operator/(T d) const {
        return Point(x / d, y / d);
    }
    T cross(const Vector b) const {
        return x * b.y - y * b.x;
    }
    T dot(const Vector b) const {
        return x * b.x + y * b.y;
    }
    T dist2() const {
        return x * x + y * y;
    }
    T dist() const {
        return sqrt((double)dist2());
    }
    T angle() const {
        return atan2(y, x); // [-pi, pi] to x_axis
    }
    T unit() const {
        return *this / dist();  // unit vector
    }
    friend istream& operator>>(istream& os, Point<T>& p) {
        return os >> p.x >> p.y;
    }
    friend ostream& operator<<(ostream& os, Point<T> p) {
        return os << '(' << p.x << ", " << p.y << ")";
    }
};

const double EPS = 1e-9;
// Orientation of 3 ordered points
int orientation(Point<double> a, Point<double> b, Point<double> c) { 
    double val = (b.y - a.y) * (c.x - b.x) - (b.x - a.x) * (c.y - b.y);
    if (fabs(val) < EPS) {
        return 0;   // collinear
    }
    return val > 0 ? 1 : 2; // clock or counterclockwise
}

// Dist from point to segment/line
double line_point_dist(Point<double> a, Point<double> b, Point<double> c, bool is_segment) {
    if (is_segment) {
        double dot1 = (a - b).dot(c - b);
        if (dot1 < 0) return (b - c).dist();
        double dot2 = (b - a).dot(c - a);
        if (dot2 < 0) return (a - c).dist();
    }
    return abs((b - a).cross(c - a)) / (a - b).dist();
}

bool on_segment(Point<double> a, Point<double> b, Point<double> c) {
    return c.x <= max(a.x, b.x) && c.x >= min(a.x, b.x) && c.y <= max(a.y, b.y) && c.y >= min(a.y, b.y);
}
bool intersect(const Point<double>& A, const Point<double>& B, const Point<double>& C, const Point<double>& D) {
    // Find the four orientations needed for the general and special cases
    int o1 = orientation(A, B, C);
    int o2 = orientation(A, B, D);
    int o3 = orientation(C, D, A);
    int o4 = orientation(C, D, B);

    // General case
    if (o1 != o2 && o3 != o4) return true;

    // Special cases (collinear points)
    // A, B, C are collinear and C lies on segment AB
    if (o1 == 0 && on_segment(A, B, C)) return true;

    // A, B, D are collinear and D lies on segment AB
    if (o2 == 0 && on_segment(A, B, D)) return true;

    // C, D, A are collinear and A lies on segment CD
    if (o3 == 0 && on_segment(C, D, A)) return true;

    // C, D, B are collinear and B lies on segment CD
    if (o4 == 0 && on_segment(C, D, B)) return true;

    return false; // Otherwise, they don't intersect
}

template<class T> T polygon_area2(vector<Point<T>>& v) {
    T ret = v.back().cross(v[0]);
    for (int i = 0; i < (int)v.size() - 1; i++) {
        ret += v[i].cross(v[i+1]);
    }
    return ret;
}

template<class T> T point_in_polygon(const vector<Point<T>>& v, const Point<T>& p) {
    bool boundary = false;
    int cnt = 0;
    for (int i = 0; i < (int)v.size(); i++) {
        int j = (i == v.size() - 1 ? 0 : i + 1);
        if (on_segment(v[i], v[j], p)) {
            boundary = true;
            break;
        }
    }
}
```