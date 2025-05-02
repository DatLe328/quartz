# Lũy thừa nhị phân

**Binary Exponentiation** là thuật toán tính lũy thừa của một số một cách hiệu quả bằng cách phân tích lũy thừa dưới dạng nhị phân.

## **Độ phức tạp**

- **Thời gian**: O(log n), với n là số mũ.
- **Không gian**: O(1), vì chỉ cần một lượng không gian cố định để lưu trữ các biến tạm thời.

## **Ứng dụng**

- **Toán học và khoa học máy tính**: Sử dụng để tính toán nhanh các lũy thừa lớn.
- **Mã hóa và bảo mật**: Sử dụng trong các thuật toán như RSA và Diffie-Hellman.
- **Đồ thị và hình ảnh**: Sử dụng trong các thuật toán liên quan đến đồ thị và hình ảnh.
https://codeforces.com/contest/1985/problem/G
## **Tính lũy thừa chia lấy dư dùng đệ quy**
```cpp
long long Pow(long long a, long long b, long long M) {
    if (!b) return 1;
    long long x = Pow(a, b / 2);
    if (b % 2 == 0)
        return (x * x) % M;
    else
        return (x * x * a) % M;
}
```
## **Tính lũy thừa chia lấy dư khử đệ quy**
```cpp
long long Pow(long long a, long long b, long long M) {
    long long ans = 1;
    while (b > 0){
        if (b % 2) ans = ans * a % M;
        a = a * a % M;
        b /= 2;
    }
    return ans;
}
```

### **Nhân lấy dư**
```cpp
long long Mul(long long a, long long b) {
    if (!b) return 0;
    long long x = Mul(a, b / 2);
    if (b % 2 == 0)
        return 2 * x % m;
    else
        return (2 * x + a) % m;
}
```

### **Dùng trong các phép lũy thừa ma trận**