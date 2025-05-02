# Kiến thức cần nắm
- [[Inversion Modulo]] 
# Hệ số nhị thức (Số tổ hợp)

**Binomial Coefficient** nCk là số cách chọn k phần tử từ n phần tử, tính bằng công thức n! / (k!(n−k)!)

Mở đầu, chúng ta sẽ tính hệ số nhị thức theo các phương pháp:

- Sử dụng định nghĩa
- Sử dụng công thức truy hồi
- Sử dụng định lý Lucas (mở rộng)

## **Độ phức tạp**

Khi tính trong modulo M, độ phức tạp của các phương pháp tính nCk ở trên như sau:

- **Sử dụng định nghĩa**
    - **Thời gian:**
        - **Tiền xử lý:** O(n^2)
        - **Truy vấn:** O(1)
    - **Không gian:** O(n^2)
- **Sử dụng công thức truy hồi**
    - **Thời gian:**
        - **Tiền xử lý:** O(n + log M)
        - **Truy vấn:** O(1)
    - **Không gian:** O(n)
- **Sử dụng định lý Lucas (mở rộng)**
    - **Thời gian:**
        - **Tiền xử lý:** O(M)
        - **Truy vấn:** O(log n)
    - **Không gian:** O(M)

## **Ứng dụng**

- **Toán học và tổ hợp**: Sử dụng trong các bài toán về xác suất, tổ hợp, và thống kê.
- **Lập trình**: Tính toán các giá trị tổ hợp trong các bài toán lập trình và giải thuật.
- **Vật lý và khoa học máy tính**: Sử dụng trong các bài toán về phân phối và phân tích dữ liệu.

```cpp
ll qexp(ll a, ll b, ll m) {
	ll res = 1;
	while (b) {
		if (b % 2) res = res * a % m;
		a = a * a % m;
		b /= 2;
	}
	return res;
}
 
ll fact[MAX_N], invf[MAX_N];
 
void precompute(int n) {
	fact[0] = 1;
	for (int i = 1; i <= n; i++) fact[i] = fact[i - 1] * i % MOD;
	invf[0] = 1;
	invf[n] = qexp(fact[n], MOD - 2, MOD);
	for (int i = n - 1; i > 0; i--) invf[i] = invf[i + 1] * (i + 1) % MOD;
}
 
ll nCk(int n, int k) {
	if (k < 0 || k > n) return 0;
	return fact[n] * invf[k] % MOD * invf[n - k] % MOD;
}
```