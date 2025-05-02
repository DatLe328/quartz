# 📄 Text Processing

| Command                            | Usage / Description                                                         |
| ---------------------------------- | --------------------------------------------------------------------------- |
| `whoami`                           | Hiển thị tên người dùng hiện tại                                            |
| `ls -la`                           | Liệt kê tất cả file/thư mục (bao gồm ẩn) kèm quyền, kích thước,...          |
| `man nmap`                         | Xem hướng dẫn sử dụng của lệnh `nmap`                                       |
| `locate nmap`                      | Tìm nhanh file hoặc đường dẫn chứa "nmap"                                   |
| `whereis nmap`                     | Tìm vị trí cài đặt (binary, source, man page) của `nmap`                    |
| `which nmap`                       | Trả về đường dẫn lệnh `nmap` nếu có trong `$PATH`                           |
| `find /home/ -type f -name A.cpp`  | Tìm file tên `A.cpp` trong thư mục `/home`                                  |
| `ps aux`                           | Xem tất cả tiến trình đang chạy (toàn hệ thống)                             |
| `grep pattern file.txt`            | Tìm dòng chứa `pattern` trong file                                          |
| `cat > file.txt`                   | Tạo file mới, ghi nội dung (ghi đè nếu đã có)                               |
| `cat >> file.txt`                  | Ghi thêm nội dung vào cuối file                                             |
| `touch newfile`                    | Tạo file rỗng mới                                                           |
| `cp old_direct new_direct`         | Sao chép file hoặc thư mục                                                  |
| `mv oldname newname`               | Đổi tên hoặc di chuyển file/thư mục                                         |
| `head -10 text.txt` / `head -n 10` | Hiển thị 10 dòng đầu của file                                               |
| `tail -2 text.txt` / `tail -n 2`   | Hiển thị 2 dòng cuối của file                                               |
| `nl text.txt`                      | Đánh số dòng của file                                                       |
| `nl text.txt \| tail -2`           | Đánh số dòng và lấy 2 dòng cuối                                             |
| `nl -ba`                           | Đánh số tất cả dòng, kể cả dòng trắng                                       |
| `nl -n ln`                         | Định dạng đánh số dòng kiểu left justified                                  |
| `nl -v 10`                         | Bắt đầu đánh số dòng từ 10                                                  |
| `sed 's/mysql/MySQL/g' filename`   | Thay "mysql" thành "MySQL" trên toàn file                                   |
| `sed 's/mysql/MySQL/2' filename`   | Chỉ thay lần xuất hiện thứ 2 trên mỗi dòng                                  |
| `sed '/^$/d' filename`             | Xoá các dòng trống                                                          |
| `more filename` / `less filename`  | Xem nội dung file theo trang. Gõ `/từ_khoá` để tìm kiếm, `n`/`N` để lặp lại |
| `wc filename`                      | Thống kê số dòng, từ, ký tự (sử dụng `-l`, `-w`, `-c`)                      |

---

# 🌐 Network Commands

| Command                   | Usage / Description                      |
| ------------------------- | ---------------------------------------- |
| `ifconfig`                | Hiển thị thông tin mạng (IP, MAC,...)    |
| `iwconfig`                | Hiển thị cấu hình mạng không dây         |
| `ifconfig eth0 new_ip`    | Đặt địa chỉ IP tĩnh cho card mạng `eth0` |
| `route add default gw IP` | Thêm default gateway                     |
| `dhclient eth0`           | Nhận IP tự động qua DHCP từ router       |
|                           |                                          |

### Cấu hình IP thủ công:
```bash
# Cấu hình IP, netmask, broadcast cho eth0
ifconfig eth0 192.168.181.115 netmask 255.255.0.0 broadcast 192.168.1.255

# Thêm default gateway
route add default gw 192.168.40.1

# Yêu cầu router cấp IP động (qua DHCP)
dhclient eth0
```

### Thay đổi MAC Address:
```bash
ifconfig eth0 down
ifconfig eth0 hw ether 00:11:22:33:44:55
ifconfig eth0 up
```