- **ICMP** (Internet Control Message Protocol)
- ICMP **không truyền dữ liệu người dùng** (như web, email)  
    → mà dùng để **trao đổi thông tin kỹ thuật giữa các thiết bị mạng**.
- Nó **không tin cậy** (không xác nhận nhận thành công), và **không dùng cổng (port)** như TCP hay UDP.

- Dùng lệnh `ping` (dựa trên ICMP): `ping 8.8.8.8`
-> Gửi gói ICMP Echo Request đến Google DNS → nếu nhận Echo Reply → máy đó **đang online**.

Dùng lệnh `traceroute` (Linux) hoặc `tracert` (Windows): `tracert 8.8.8.8`
-> Gửi nhiều gói ICMP với TTL tăng dần → hiển thị **đường đi của gói tin** đến máy đích.