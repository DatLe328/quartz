# Chia căn

**Chia căn** là tên gọi chung của một nhóm các thuật toán thường liên quan đến việc chia các đối tượng thành sqrt(N) phần. Bằng cách này, trong nhiều trường hợp ta sẽ giảm được độ phức tạp của xử lý dữ liệu từ O(N) xuống còn O(sqrt(N))
# Blocks/Buckets

- Kỹ thuật chia dữ liệu thành các **khối (blocks)** hoặc các **nhóm (buckets)** để xử lý truy vấn và cập nhật hiệu quả hơn.
- Sử dụng trong các bài toán với dữ liệu lớn để giảm độ phức tạp của truy vấn.

**Trong thuật toán chia căn cơ bản,** ta sẽ tối ưu hóa các truy vấn trên mảng bằng cách chia mảng thành các khối có kích thước xấp xỉ căn bậc hai của kích thước mảng. Điều này giúp giảm độ phức tạp của truy vấn và cập nhật.

## Độ phức tạp

Trong một số trường hợp, khi chia được mảng thành các khối có kích thước xấp xỉ căn bậc hai, độ phức tạp sẽ giảm xuống O(sqrt(N)) trong việc thao tác với các phần tử

## Ứng dụng

- Các bài toán truy vấn tổng đoạn, tìm min/max đoạn.
- Giải các bài toán về truy vấn và cập nhật trên mảng hoặc chuỗi.
- Tối ưu hóa độ phức tạp thời gian của các thao tác.

```cpp
template<typename T>
class SQRT {
private:
    const static int BLOCK_SIZE = 500;
    int n;
    vector<T> buc, arr;
public:
    SQRT() {}
    SQRT(const vector<T>& a) : n(a.size()), buc(n / BLOCK_SIZE + 5), arr(n) {
        int n = int(arr.size());
        for (int i = 0; i < n; i++) {
            arr[i] = a[i];
            buc[i / BLOCK_SIZE] += arr[i];
        }
    }
    void update(int idx, T val) {
        buc[idx / BLOCK_SIZE] += val - arr[idx];
        arr[idx] = val;
    }
    T query(int l, int r) {
        T ret = 0;
        while (l % BLOCK_SIZE != 0 && l < r) {
            ret += arr[l];
            l++;
        }
        while (l + BLOCK_SIZE <= r) {
            ret += buc[l / BLOCK_SIZE];
            l += BLOCK_SIZE;
        }
        while (l <= r) {
            ret += arr[l];
            l++;
        }
        return ret;
    }
};

```