![[how_address_resolution_protocol_works-f.png]]
- Máy tính chỉ có IP nhưng để truyền dữ liệu trên mạng LAN, nó cần biết **địa chỉ MAC** của máy đích.  
- ARP giúp nó **hỏi xem IP này có MAC là gì**, rồi lưu lại để dùng về sau.

- Bạn có thể xem bảng ARP trên máy mình:
	- **Windows**: `arp -a`
	- **Linux / macOS**: `ip neigh`, `arp -n`

ARP không có cơ chế xác thực → có thể bị **ARP spoofing / poisoning**  
→ hacker có thể **giả mạo MAC**, đánh lừa máy khác, phục vụ tấn công MITM (Man-in-the-Middle).