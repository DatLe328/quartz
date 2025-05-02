![[TCP-UDP.png]]

| Giao thức                               | Đặc điểm chính                                       | Ví dụ dễ hiểu                                                                  |
| --------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------------------ |
| **TCP** (Transmission Control Protocol) | **Đảm bảo độ tin cậy, có kiểm tra lỗi, có xác nhận** | Giống gửi thư bảo đảm – có biên nhận, đảm bảo đến đúng người, đúng thứ tự      |
| **UDP** (User Datagram Protocol)        | **Nhanh, không đảm bảo, không xác nhận**             | Giống hét qua loa – ai nghe được thì nghe, không kiểm tra xem có ai nghe không |

| Tiêu chí                   | **TCP**                                                    | **UDP**                                                      |
| -------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------ |
| **Kết nối**                | Có thiết lập kết nối (3 bước bắt tay - handshake)          | Không thiết lập kết nối                                      |
| **Đảm bảo truyền dữ liệu** | Có: đảm bảo toàn vẹn, đúng thứ tự, không mất dữ liệu       | Không: không đảm bảo gì cả                                   |
| **Tốc độ**                 | Chậm hơn (vì kiểm tra nhiều)                               | Nhanh hơn (ít kiểm tra)                                      |
| **Truyền lại gói tin lỗi** | Có                                                         | Không                                                        |
| **Thứ tự gói tin**         | Được đảm bảo                                               | Không đảm bảo                                                |
| **Kiểm tra lỗi**           | Có CRC + xác nhận                                          | Có CRC nhưng không bắt buộc xử lý lại                        |
| **Dùng cho ứng dụng**      | Cần độ tin cậy cao: Web (HTTP/HTTPS), Email (SMTP), FTP... | Cần tốc độ, chịu mất mát: Video call, VoIP, Game online, DNS |
| **Độ phức tạp**            | Phức tạp hơn                                               | Đơn giản hơn                                                 |
| **Sử dụng tài nguyên**     | Tốn RAM/CPU hơn (vì lưu trạng thái)                        | Ít tài nguyên hơn                                            |
| **Ví dụ lệnh Linux**       | `curl http://...` (dùng TCP)                               | `dig example.com` (DNS dùng UDP)                             |

- Quy trình bắt tay 3 bước:
	- **Bước 1**: Máy chủ A khởi tạo kết nối bằng cách gửi gói TCP & đến máy chủ đích. Gói chứa số thứ tự ngẫu nhiên (ví dụ: 5432 ) đánh dấu sự bắt đầu của số thứ tự cho dữ liệu mà Máy chủ A sẽ truyền.
	- **Bước 2**: Máy chủ nhận gói và phản hồi bằng số thứ tự của chính nó. Phản hồi cũng bao gồm số xác nhận, là số thứ tự của Máy chủ A được tăng thêm 1 (Ví dụ như: 5433 ).
	- **Bước 3**: Máy chủ A xác nhận phản hồi của Máy chủ bằng cách gửi số xác nhận, là số thứ tự của Máy chủ tăng thêm 1.
	
![[Pasted image 20250430200813.png]]

|Step|ACK|SYN|Seq number|Ack number|
|---|---|---|---|---|
|Step 1|0|1|1000|0|
|Step 2|1|1|1500|1001|
|Step 3|1|0|1001|1501|
