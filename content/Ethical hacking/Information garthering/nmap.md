`-vv` : **Very verbose**
`--open`

-sS: instead of three-way handshake like this SYN SYNACK ACK
nmap will use RST (reset) flag to replace the last
-T4 : we can change number from 1 to 5 (slow-fast)
-p- : i want to scan all port, if we don't use this flag, normally it will scan
top 1000 most use port
-A : everything about os, version, service
# Target selection
- Scan a single IP/Host
```bash
nmap scanme.nmap.org
nmap 192.168.0.0
```
- Scan a range of IPs
```bash
nmap 192.168.0.0-10
```
- Scan targets from a text file
```bash
nmap -iL list-of-ips.txt
```
# Port Selection (-p)
- Scan a single Port
```bash
nmap -p 22 192.168.1.1
```
- Scan a range of ports
```bash
nmap -p 1-100 192.168.1.1
```
- Scan 100 most common ports (Fast)
```bash
nmap -F 192.168.1.1
```
- Scan all 65535 ports
```bash
nmap -p 22 192.168.1.1
```
# Port Scan types (-s)
- Scan using TCP connect
```bash
nmap -sT 192.168.1.1
```
- Scan using TCP SYN scan (default)
```bash
nmap -sS 192.168.1.1
```
- Scan UDP ports
```bash
nmap -sU -p 123,161,162 192.168.1.1
```
- Scan selected ports - ignore discovery
```bash
nmap -Pn -F 192.168.1.1
```
# Output Formats (-o)
- Save default output to file
```bash
nmap -oN outputfile.txt 192.168.1.1
```
- Save results as XML
```bash
nmap -oX outputfile.xml 192.168.1.1
```
- Save results in a format for grep
```bash
nmap -oG outputfile.txt 192.168.1.1
```
- Save in all formats
```bash
nmap -oA outputfile 192.168.1.1
```
# Service and OS Detection
- Aggressive Scanning

> nmap -A <IP/Host name>
```bash
nmap -A 192.168.40.100
```
- Standard service detection (Chỉ phát hiện dịch vụ + phiên bản)
```bash
nmap -sV 192.168.40.100
```
- **-O** (detect OS) **require root permisson**
```bash
nmap -sV 192.168.40.100
```
- `-A` dễ bị **phát hiện** bởi firewall hoặc IDS (hệ thống phát hiện xâm nhập) vì nó "spam" rất nhiều gói tin lạ.
- Nếu bạn muốn stealth (ẩn mình) hơn, nên tự chọn các options nhỏ thay vì quất nguyên `-A`.

| So sánh      | -sV                                  | -A                                                                 |
|--------------|--------------------------------------|---------------------------------------------------------------------|
| Công việc    | Chỉ phát hiện dịch vụ + phiên bản     | Phát hiện dịch vụ + phiên bản + OS + traceroute + chạy script       |
| Mức độ       | Nhẹ hơn                               | Rất nặng (scan kỹ)                                                  |
| Khi nào dùng | Khi bạn chỉ cần biết service          | Khi bạn cần toàn bộ thông tin chi tiết về máy đích                    |
| Thời gian scan| Nhanh hơn                             | Chậm hơn, có thể lâu                                                 |