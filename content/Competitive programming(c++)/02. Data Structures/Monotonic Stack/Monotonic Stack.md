# Ngăn xếp (Stack)

**Ngăn xếp (Stack)** là một cấu trúc dữ liệu tuyến tính hoạt động theo nguyên tắc "Last In, First Out" (LIFO), tức là phần tử được thêm vào cuối cùng sẽ được lấy ra đầu tiên.

## **Một số thao tác và Độ phức tạp**

1. **Push:** Thêm một phần tử vào đỉnh ngăn xếp trong thời gian O(1).
2. **Pop:** Loại bỏ và trả về phần tử ở đỉnh ngăn xếp trong thời gian O(1).
3. **Peek/Top:** Trả về phần tử ở đỉnh ngăn xếp mà không loại bỏ nó trong thời gian O(1).
4. **IsEmpty:** Kiểm tra xem ngăn xếp có rỗng hay không trong thời gian O(1).

#### **Độ phức tạp không gian**: O(n)

## Ứng dụng

- **Quản lý gọi hàm**: Dùng trong quản lý stack gọi hàm trong lập trình. Một ví dụ điển hình là các mức đệ quy của hàm được lưu trong stack.
- **Xử lý biểu thức**: Được sử dụng trong chuyển đổi và tính toán biểu thức trung tố, hậu tố.
- **Monotonic stack** (stack đơn điệu)

## **Monotonic Stack**
> Dùng để kiểm tra phần tử nào lớn hơn gần nhất bên trái
```cpp
stack<int> st;

for (int i = 1; i <= n; ++i)
{
    while (!st.empty() && a[st.top()] <= a[i])
        st.pop();
    int ans = -1;
    if (!st.empty())
        ans = st.top();
    cout << ans << ' ';
    st.push(i);
}
```
## **Min/Max stack**
>Đùng để tìm giá trị lớn nhất và giá trị nhỏ nhất của một dãy
```cpp
struct Stack {
    vector<int64_t> s, mn{LLONG_MAX}, mx{LLONG_MIN};
    void push(int64_t x) {
        s.push_back(x);
        mn.push_back(::min(mn.back(), x));
        mx.push_back(::max(mx.back(), x));
    }
    int64_t pop() {
        int64_t ret = s.back();
        s.pop_back();
        mn.pop_back();
        mx.pop_back();
        return ret;
    }
    int64_t min() {
        return mn.back();
    }
    int64_t max() {
        return mx.back();
    }
    bool empty() {
        return s.empty();
    }
};

Stack s1, s2;
void add(int64_t x) {
    s2.push(x);
}
void remove() {
    if (s1.empty()) {
        while (!s2.empty()) {
            s1.push(s2.pop());
        }
    }
    s1.pop();
}

int64_t max_val() {
	return max(s1.max(), s2.max());
}

int64_t min_val() {
	return min(s1.min(), s2.min());
}
```

## **GCD stack**
>Dùng để tìm gcd của một dãy
```cpp
struct Stack {
    vector<int64_t> s, v {0};
    void push(int64_t x) {
        s.push_back(x);
        v.push_back(__gcd(v.back(), x));
    }
    int64_t pop() {
        int64_t ret = s.back();
        s.pop_back();
        v.pop_back();
        return ret;
    }
    int64_t top() {
        return v.back();
    }
    bool empty() {
        return s.empty();
    }
};

Stack s1, s2;
void add(int64_t x) {
    s2.push(x);
}
void remove() {
    if (s1.empty()) {
        while (!s2.empty()) {
            s1.push(s2.pop());
        }
    }
    s1.pop();
}

bool good() {
    return __gcd(s1.top(), s2.top()) == 1;
}
```