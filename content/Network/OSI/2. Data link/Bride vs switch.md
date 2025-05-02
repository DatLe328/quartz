| Tiêu chí                  | 🟦 **Bridge**                                                        | 🟩 **Switch**                                                        |
| ------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| **Vị trí OSI**            | Tầng 2 – Data Link                                                   | Tầng 2 – Data Link (một số switch thông minh còn hỗ trợ tầng 3)      |
| **Chức năng chính**       | Kết nối 2 mạng LAN để mở rộng mạng                                   | Kết nối nhiều thiết bị trong 1 LAN, chuyển tiếp dữ liệu thông minh   |
| **Số cổng**               | Thường chỉ 2 hoặc 3 cổng                                             | Từ vài cổng đến hàng chục cổng (4–48+)                               |
| **Cách học địa chỉ MAC**  | Lưu MAC của thiết bị mỗi bên mạng → quyết định chuyển tiếp hay không | Lưu MAC cho từng cổng → gửi trực tiếp đến cổng đích                  |
| **Broadcast domain**      | Mỗi bridge là 1 broadcast domain                                     | Toàn bộ switch nằm trong cùng 1 broadcast domain (trừ khi dùng VLAN) |
| **Collision domain**      | Bridge chia thành 2 collision domain riêng biệt                      | Mỗi cổng là 1 collision domain → giảm xung đột mạng                  |
| **Tốc độ xử lý dữ liệu**  | Tương đối chậm                                                       | Rất nhanh (switch hoạt động song song nhiều cổng)                    |
| **Hiệu suất mạng**        | Thấp hơn switch                                                      | Cao hơn nhiều nhờ switching logic                                    |
| **Chế độ truyền dữ liệu** | Half duplex                                                          | Full duplex (đa phần switch hiện đại)                                |
| **Hỗ trợ VLAN**           | Không hỗ trợ                                                         | Có (nếu là managed switch)                                           |
| **Dễ mở rộng mạng**       | Không tiện khi nhiều thiết bị                                        | Rất linh hoạt và mở rộng dễ dàng                                     |
| **Sử dụng trong thực tế** | Cũ, ít dùng, chủ yếu trong các ví dụ giáo dục                        | Rất phổ biến trong mạng doanh nghiệp và gia đình                     |
| **Giá thành (trước đây)** | Từng rẻ hơn switch                                                   | Nay rất rẻ và phổ biến hơn bridge                                    |