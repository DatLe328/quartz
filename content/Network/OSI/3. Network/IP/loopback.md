- **Địa chỉ loopback** là địa chỉ IP đặc biệt được dùng để **thiết bị tự giao tiếp với chính nó**. Nó còn gọi là **localhost**, thường dùng để kiểm tra phần mềm hoặc dịch vụ **trên chính máy tính đang chạy**, mà không cần truy cập ra mạng bên ngoài.

- **IPv4 loopback**: `127.0.0.1` (phổ biến nhất)
- Toàn bộ dải `127.0.0.0/8` đều là loopback  
    → tức là từ `127.0.0.1` đến `127.255.255.254` đều là địa chỉ loopback
- **IPv6 loopback**: `::1`

| Mục đích                            | Giải thích                                                                        |
| ----------------------------------- | --------------------------------------------------------------------------------- |
| ✅ **Kiểm tra phần mềm/mạng cục bộ** | Ví dụ chạy một server web tại `127.0.0.1:8000`, chỉ bạn trên máy đó truy cập được |
| ✅ **Không cần card mạng**           | Kể cả khi không có kết nối mạng, loopback vẫn hoạt động                           |
| ✅ **Kiểm tra dịch vụ**              | Kiểm tra xem ứng dụng, dịch vụ nội bộ có hoạt động hay không                      |
| ✅ **Phát triển ứng dụng**           | Lập trình viên thường dùng loopback để test app trước khi triển khai thực tế      |