**CIDR** (viết tắt của **Classless Inter-Domain Routing**) là **một cách viết địa chỉ IP kèm độ dài phần mạng**, giúp xác định chính xác đâu là phần mạng và đâu là phần host trong địa 
chỉ IP.

**Ví dụ:**
- `192.168.1.0/24` → phần mạng dài 24 bit → còn 8 bit dành cho host
- `10.0.0.0/8` → phần mạng dài 8 bit → còn 24 bit cho host

| CIDR | Subnet Mask     | Số địa chỉ IP | Số IP usable |
| ---- | --------------- | ------------- | ------------ |
| /8   | 255.0.0.0       | 16,777,216    | ~16 triệu    |
| /16  | 255.255.0.0     | 65,536        | ~65 nghìn    |
| /24  | 255.255.255.0   | 256           | 254          |
| /26  | 255.255.255.192 | 64            | 62           |
| /30  | 255.255.255.252 | 4             | 2            |
