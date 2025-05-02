## Kiến thức cần thiết [[Monotonic Stack]] dùng để truy vấn min/max
# Hai con trỏ

**Kỹ thuật hai con trỏ (Two Pointers)** là một phương pháp thường được sử dụng trong lập trình và giải thuật để xử lý các vấn đề liên quan đến mảng hoặc chuỗi. Kỹ thuật này sử dụng hai con trỏ để duyệt qua cấu trúc dữ liệu, thường với các mục tiêu khác nhau như tìm kiếm, sắp xếp hoặc tối ưu hóa.

## **Độ phức tạp**

- **Thời gian**: O(n)
- **Không gian**: O(1)

## I. Segment with good sum
**Đề bài:** Cho một dãy số và một giá trị s:
- Tìm đoạn dài nhất sao cho tổng <= s
- Tìm đoạn ngắn nhất sao cho tổng >= s
- Đếm số đoạn mà tổng của chúng <= s
- Đếm số đoạn mà tổng của chúng >= s
[Đề bài](https://codeforces.com/edu/course/2/lesson/9/2/practice)
```cpp
int cur = 0, sum = 0;
int ans = 0;
for (int i = 0; i < n; i++) {
	sum += a[i];
	while (sum > s) {
		sum -= a[cur];
		cur++;
	}
	/* Đoạn ngắn nhất sum >= s
	while (sum - a[cur] >= s) {
		sum -= a[cur];
		cur++;
	}
	*/
	ans = max(ans, i - cur + 1);
	
	/* Kết quả đoạn ngắn nhất sum <= s
	ans = min(ans, i - cur + 1);
	*/
}
```

```cpp
int cur = 0, sum = 0;
int ans = 0;
for (int i = 0; i < n; i++) {
	sum += a[i];
	while (sum > s) {
		sum -= a[cur];
		cur++;
	}
	/* Số đoạn sum >= s
	while (sum - a[cur] >= s) {
		sum -= a[cur];
		cur++;
	}
	*/
	ans += i - cur + 1;
	
	/* Số đoạn sum >= s
	ans += cur;
	*/
}
```
### Tổng quát hơn nếu ta muốn giải quyết các bài toán dùng hai con trỏ
```
L = 0
for R = 0..n-1
    add(a[R])
    while not good():
        remove(a[L])
        L++
```
> Ta cần triển khai hàm **add, good, remove**
