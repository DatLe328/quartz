# Thuật toán Kruskal

**Thuật toán Kruskal** là một thuật toán để tìm cây khung nhỏ nhất của đồ thị. Thuật toán này hoạt động bằng cách sắp xếp các cạnh theo trọng số tăng dần và chọn các cạnh nhỏ nhất sao cho không tạo thành chu trình.

## Cách hoạt động

1. Sắp xếp các cạnh theo trọng số tăng dần.
2. Khởi tạo DSU cho các đỉnh.
3. Duyệt qua các cạnh đã sắp xếp, thêm cạnh vào MST nếu hai đỉnh của cạnh không thuộc cùng một tập hợp (sử dụng DSU).

## Độ phức tạp

- **Thời gian:** O(Elog(E))
- **Không gian:** O(E)

## Ứng dụng

- Tìm MST cho các đồ thị thưa.

# Thuật toán Prim

**Thuật toán Prim** cũng là một thuật toán để tìm cây khung nhỏ nhất. Thuật toán này bắt đầu từ một đỉnh và mở rộng cây khung bằng cách thêm các cạnh có trọng số nhỏ nhất nối các đỉnh chưa được ghé thăm.

## Cách hoạt động

1. Chọn một đỉnh bắt đầu.
2. Sử dụng cấu trúc dữ liệu ưu tiên (heap/priority queue) để luôn chọn cạnh có trọng số nhỏ nhất nối cây khung với một đỉnh mới.
3. Lặp lại cho đến khi tất cả các đỉnh được bao phủ.

## Độ phức tạp

- **Thời gian:** O(E log⁡ V)
- **Không gian:** O(E)

## Ứng dụng

- Tìm MST cho các đồ thị dày đặc.

# Thuật toán Boruvka(Đọc thêm)

- **Boruvka** là một trong những thuật toán đầu tiên được phát triển để tìm cây khung nhỏ nhất.
- Thuật toán liên tục chọn cạnh nhỏ nhất từ mỗi thành phần con và hợp nhất các thành phần này lại.

## Cách hoạt động

1. Bắt đầu với mỗi đỉnh là một thành phần con riêng lẻ.
2. Trong mỗi bước, chọn cạnh nhỏ nhất nối mỗi thành phần con với một thành phần khác.
3. Hợp nhất các thành phần con lại bằng các cạnh vừa chọn.
4. Lặp lại cho đến khi tất cả các thành phần được hợp nhất thành một cây.

## Độ phức tạp

- **Thời gian**: O(E log⁡ V)
- **Không gian**: O(E)

- [ARTICLECodeforces - [](https://codeforces.com/blog/entry/77760)


## **Bài tập**
[Mahmoud and Ehab and the xor-MST](https://codeforces.com/problemset/problem/959/E)