# Bitset

**Bitset:**

- Là một mảng các bit, hỗ trợ thao tác bit hiệu quả.
    
- Kích thước cố định, lưu trữ hiệu quả về mặt bộ nhớ.
    

## **Một số thao tác và Độ phức tạp**

Tùy thuộc vào phần cứng, kích thước của một từ máy có thể là 32bit hoặc 64bit. Do đó, độ phức tạp một số thao tác bitset trên một chuỗi n bits là:

1. **Khởi tạo**:
    
    - Khởi tạo từ một chuỗi ký tự: `bitset<8> b("10101010");`
    - Khởi tạo từ số nguyên: `bitset<8> b(170);`
    - Độ phức tạp: O(n/32) hoặc O(n/64).
2. **Toán tử** `[]`:
    
    - Truy cập hoặc thay đổi giá trị của bit tại vị trí cụ thể: `b[2] = 1;`
    - Độ phức tạp: O(1).
3. `set`:
    
    - Đặt tất cả các bit hoặc một bit cụ thể thành 1: `b.set();` hoặc `b.set(pos);`
    - Độ phức tạp:
        - O(n/32) hoặc O(n/64) cho `b.set()`
        - O(1) cho `b.set(pos)`.
4. `reset`:
    
    - Đặt tất cả các bit hoặc một bit cụ thể thành 0: `b.reset();` hoặc `b.reset(pos);`
    - Độ phức tạp:
        - O(n/32) hoặc O(n/64) cho `b.reset()`
        - O(1) cho `b.reset(pos)`.
5. `flip`:
    
    - Đảo ngược tất cả các bit hoặc một bit cụ thể: `b.flip();` hoặc `b.flip(pos);`
    - Độ phức tạp:
        - O(n/32) hoặc O(n/64) cho `b.flip()`
        - O(1) cho `b.flip(pos)`.
6. `count`:
    
    - Đếm số bit có giá trị 1: `b.count();`
    - Độ phức tạp: O(n/32) hoặc O(n/64).
7. `any`:
    
    - Kiểm tra xem có bit nào có giá trị 1 không: `b.any();`
    - Độ phức tạp: O(n/32) hoặc O(n/64).
8. `none`:
    
    - Kiểm tra xem tất cả các bit có giá trị 0 không: `b.none();`
    - Độ phức tạp: O(n/32) hoặc O(n/64)
9. `all`:
    
    - Kiểm tra xem tất cả các bit có giá trị 1 không: `b.all();`
    - Độ phức tạp: O(n/32) hoặc O(n/64).
10. **Toán tử bitwise**:
    
    - AND, OR, XOR: `b1 & b2`, `b1 | b2`, `b1 ^ b2`
    - Độ phức tạp: O(n/32) hoặc O(n/64).
11. `to_string`:
    
    - Chuyển đổi bitset thành chuỗi ký tự: `b.to_string();`
    - Độ phức tạp: O(n).
12. `to_ulong` và `to_ullong`:
    
    - Chuyển đổi bitset thành số nguyên không dấu: `b.to_ulong();` hoặc `b.to_ullong();`
    - Độ phức tạp: O(1).

**Độ phức tạp không gian:** O(n/8)

## **Ứng dụng:**

- Làm việc với các bài toán yêu cầu thao tác bit nhanh.
- Sử dụng trong các bài toán cần lưu trữ nhiều giá trị nhị phân.