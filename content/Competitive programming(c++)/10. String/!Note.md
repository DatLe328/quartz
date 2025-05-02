| **Bài toán**                           | **Suffix Array**         | **Suffix Automaton**           |
| -------------------------------------- | ------------------------ | ------------------------------ |
| **Tìm substring**                      | Tốt cho nhiều truy vấn   | Tốt cho kiểm tra nhanh         |
| **Đếm chuỗi con phân biệt**            | Có thể, cần thêm LCP     | Hiệu quả và nhanh hơn          |
| **Tìm số lần xuất hiện của substring** | Không trực tiếp          | Trực tiếp và nhanh             |
| **Tìm chuỗi con lặp lại dài nhất**     | Dựa vào LCP              | Tìm trạng thái với `count > 1` |
| **Tìm chuỗi con chung**                | Có thể, cần bổ sung thêm | Tốt khi xử lý nhiều chuỗi      |
| **Sắp xếp từ điển các substring**      | Trực tiếp                | Không trực tiếp                |
| **Truy vấn động**                      | Không hỗ trợ             | Hỗ trợ động                    |
| **Tìm K-th substring**                 | Khó hơn                  | Dễ dàng nhờ thuộc tính đếm     |

