- Phân vùng ZTF và UFS:
  - Nếu máy có tối thiểu 8gb RAM thì nên dùng ZTF
  - Còn nếu máy cũ và RAM ít hơn thì dùng UFS
  - Lí do: Vì ZTF có tích hợp volume manager, RAID-Z, mirroring, tự động checksum và sửa lỗi silent birot, Compression & Deduplication, phân vùng linh hoạt, hiệu năng cao còn UFS thì ngược lại
- UFS Paritition scheme

| Tên | Viết tắt             | Mô tả                               | Hỗ trợ hệ thống                                         |
| --- | -------------------- | ----------------------------------- | ------------------------------------------------------- |
| APM | Apple Partition Map  | Dành cho máy Mac PowerPC cũ         | Chỉ máy Mac cổ                                          |
| BSD | BSD Disklabel        | Cách phân vùng truyền thống của BSD | Chỉ FreeBSD, không boot trực tiếp được nếu không có MBR |
| GPT | GUID Partition Table | Hiện đại, hỗ trợ UEFI, >2TB         | BIOS + UEFI (mới)                                       |
| MBR | Master Boot Record   | Cũ, giới hạn 2TB                    | BIOS (legacy)                                           |


- Trong quá trình thiết lập môi truờng lab:
  - Cần kiểm tra xem pfsense đã bật DHCP ở mạng LAN chưa
  - Kiểm tra mạng LAN của pfsense có nằm trong dãy ip mà card mạng cung cấp không
  - Ở các máy nằm trong LAN cần phải release ip cũ để nhận ip mới từ pfsense
  - Tắt DHCP ở card mạng của vmware để pfsense có thể cấp ip
