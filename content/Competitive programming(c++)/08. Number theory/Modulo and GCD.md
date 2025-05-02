- **Modulo** là phép toán lấy phần dư của phép chia. Ví dụ, a modulo  b (ký hiệu là **a mod b**) là phần dư khi chia a cho b.
    
- **GCD (Greatest Common Divisor)** là ước chung lớn nhất của hai số nguyên.
    
    Thuật toán Euclid là cách hiệu quả và phổ biến nhất để tìm GCD. Ngoài ra, để tối ưu trong lập trình thì thuật toán Euclid được cải tiến thành thuật toán Binary GCD.
    

## **Độ phức tạp tính GCD**

- **Thời gian**: Cả hai thuật toán đều cho độ phức tạp O(log min(a, b)) khi tính gcd(a, b).
- **Không gian**: O(1), vì chỉ cần một lượng không gian cố định để lưu trữ các biến tạm thời.

## **Ứng dụng**

- **Toán học**: Sử dụng trong các bài toán về phân số, phân tích số nguyên, và lý thuyết số.
- **Mã hóa và bảo mật**: GCD được sử dụng trong thuật toán RSA và các phương pháp mã hóa khác.
- **Thuật toán và lập trình**: Sử dụng để tối giản các phân số và giải quyết các bài toán liên quan đến ước số chung.

## **Thuật toán Euclid**
```cpp
int gcd(int A, int B) { 
	if (B == 0) 
		return A; 
	else 
		return gcd(B, A % B); 
}
```

## **Thuật toán Euclid mở rộng**
```cpp
int extended_gcd(int a, int b, int& x, int &y) {
    if (b == 0) {
        x = 1;
        y = 0;
        return a;
    }
    int x1, y1;
    int d = extended_gcd(b, a % b, x1, y1);
    x = y1;
    y = x1 - y1 * (a / b);
    return d;
}
```