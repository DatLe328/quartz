# Thành Phần Liên Thông Mạnh (Strongly Connected Components - SCC)

SCC là các tập hợp con của **đồ thị có hướng** sao cho từ bất kỳ đỉnh nào trong tập hợp có thể đi tới bất kỳ đỉnh nào khác trong cùng tập hợp.

Để tìm các thành phần mạnh liên thông (SCC), có thể sử dụng các thuật toán sau:

- **Kosaraju's Algorithm**: Sử dụng hai lần duyệt đồ thị, một lần trên đồ thị gốc và một lần trên đồ thị chuyển vị.
- **Tarjan's Algorithm**: Dựa trên DFS và sử dụng chỉ số thấp để xác định các SCC.

## Độ phức tạp

- **Thời gian**: O(V + E) (thuật toán Tarjan hoặc Kosaraju)
- **Không gian**: O(V)

## Ứng dụng

- **Phân tích đồ thị**: Xác định các thành phần liên thông mạnh trong đồ thị.
- **Lập kế hoạch**: Tìm các nhóm công việc có mối quan hệ chặt chẽ.
