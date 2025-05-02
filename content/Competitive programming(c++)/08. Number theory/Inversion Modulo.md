## **Kiến thức cần nắm**
- [[Modulo and GCD]]


**Inversion Modulo** của một số a modulo m là số b sao cho (a * b) % m = 1. Nghịch đảo modulo tồn tại nếu và chỉ nếu a và m là hai số nguyên tố cùng nhau (GCD(a, m) = 1).

Có hai thuật toán có thể sử dụng để tìm inversion modulo:

- Thuật toán lũy thừa nhanh (kết hợp với định lý Fermat hoặc Euler)
- Thuật toán Euclid mở rộng

## Độ phức tạp

- **Thời gian**: Cả hai thuật toán đều có độ phức tạp trung bình O(log m)
- **Không gian**: O(1), vì chỉ cần một lượng không gian cố định để lưu trữ các biến tạm thời.

## Ứng dụng

- **Mã hóa và bảo mật**: Sử dụng trong các thuật toán như RSA, nơi cần tìm khóa giải mã.
- **Toán học và lý thuyết số**: Giải quyết các phương trình đồng dư và các bài toán liên quan đến số học modulo.
- **Lập trình**: Sử dụng trong các bài toán về mã hóa và giải mã dữ liệu.

## **Tìm Modular inverse bằng thuật toán Extended Euclidean**
```cpp
int inv(int a, int m) {
	int x, y;
	int g = extended_euclidean(a, m, x, y);
	if (g != 1) {
	    cout << "No solution!";
	}
	else {
	    x = (x % m + m) % m;
	    cout << x << endl;
	}
}
```

## **Tìm Modular inverse bằng thuật toán Binary Exponentiation**

## **Tìm Modular inverse bằng Euclidean Division**
