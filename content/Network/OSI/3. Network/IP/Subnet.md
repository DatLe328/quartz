**Subnet** (viết tắt của _subnetwork_) là một phần nhỏ hơn được tách ra từ một mạng IP lớn hơn. Việc chia subnet giúp **quản lý, tổ chức và tối ưu hóa** việc sử dụng địa chỉ IP cũng như **cải thiện bảo mật và hiệu suất mạng**.

**Ví dụ:** Xét địa chỉ mạng *192.168.1.0/26*, số subnet có thể chia là bao nhiêu?

- Một địa chỉ IPv4 có **32 bit**, ở đây có **26 bit phần net** và **8 bit phần host**
- **Số bit mượn** là 26 - 24 = 2
$$Số\space bit \space mượn = Số \space bit \space phần \space net  - Số \space bit \space của \space lớp$$
- **Số subnet** có thể có là $2^2=4$
$$Số \space subnet=2^{số \space bit \space mượn}$$
- **Số host** là $2^{32-26}-2=62$ 
>Trừ 2 vì:
 -1 địa chỉ dùng làm **network address** (192.168.1.0)
 -1 địa chỉ dùng làm **broadcast address** (192.168.1.63)


- Danh sách các subnet tạo được

| Subnet số | Network Address    | Dải IP có thể dùng cho host       | Broadcast Address |
| --------- | ------------------ | --------------------------------- | ----------------- |
| 1         | `192.168.1.0/26`   | `192.168.1.1` – `192.168.1.62`    | `192.168.1.63`    |
| 2         | `192.168.1.64/26`  | `192.168.1.65` – `192.168.1.126`  | `192.168.1.127`   |
| 3         | `192.168.1.128/26` | `192.168.1.129` – `192.168.1.190` | `192.168.1.191`   |
| 4         | `192.168.1.192/26` | `192.168.1.193` – `192.168.1.254` | `192.168.1.255`   |

- Số subnet có thể có khi chia `192.168.1.0/246` là: `4 subnet`
- Mỗi subnet có **64 địa chỉ**, với **62 địa chỉ usable** cho host.