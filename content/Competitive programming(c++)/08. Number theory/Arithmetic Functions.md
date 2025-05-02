# Hàm số học (Arithmetic Functions)

**Hàm số học** là các hàm ánh xạ từ tập hợp các số nguyên dương vào tập hợp các số thực hoặc số nguyên, thường được sử dụng trong lý thuyết số. Ví dụ, hàm đếm số ước τ(n), hàm tổng các ước σ(n), hàm phi Euler φ(n).
(Chữ τ ( đọc là _tau_).


## **Bài toán đếm số ước**\

```cpp
int divCount(long long n) {
    int ret = 0;
    for (int i = 1; 1ll * i * i <= n; i ++) {
        if (n % i == 0) {
	        ret += 2;
	        if (i * i == n) {
		        ret--;
	        }
        }
    }
    return ret;
}
```
- Độ phức tập của thuật toán trên là sqrt(N), phù hợp cho các bài toán với N <= 10<sup>12</sup>
- Thuật toán trên còn thể cải tiến lên tới độ phức tạp căn bậc 3 của N phù hợp cho n <= 10<sup>18</sup> với thuật toán [Rabin-miller](https://wiki.vnoi.info/algo/algebra/primality_check.md#3-thu%E1%BA%ADt-to%C3%A1n-rabin-miller)
