https://codeforces.com/contest/1985/problem/H1
Bạn muốn ánh xạ nó thành một mảng một chiều b có kích thước m * n. Để truy cập phần tử ở vị trí (i, j) của mảng hai chiều bằng chỉ số một chiều, bạn dùng công thức sau:

index=i×n+j
Trong đó:

i là chỉ số hàng (row index),
j là chỉ số cột (column index),
n là số cột (columns) trong mảng hai chiều.
Ví dụ
Giả sử bạn có một mảng hai chiều a[3][4] với 3 hàng và 4 cột:

css
Sao chép mã
a = [ [a[0][0], a[0][1], a[0][2], a[0][3]],
      [a[1][0], a[1][1], a[1][2], a[1][3]],
      [a[2][0], a[2][1], a[2][2], a[2][3]] ]
Bạn có thể lưu nó thành mảng một chiều b[12]. Các phần tử được ánh xạ như sau:

b[0] = a[0][0]
b[1] = a[0][1]
b[2] = a[0][2]
b[3] = a[0][3]
b[4] = a[1][0]
b[5] = a[1][1]
b[6] = a[1][2]
b[7] = a[1][3]
b[8] = a[2][0]
b[9] = a[2][1]
b[10] = a[2][2]
b[11] = a[2][3
