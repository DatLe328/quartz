
|      One-Liner Code       |                    Function                    | Example |
| :-----------------------: | :--------------------------------------------: | :-----: |
|         ch \| ‘ ‘         |  Upper case English alphabet ch to lower case  |         |
|         ch & ‘_’          |  Lower case English alphabet ch to upper case  |         |
|    n & ~(1 << (k - 1))    |          Turn Off kth bit in a number          |         |
|    n \| (1 << (k - 1))    |          Turn On kth bit in a number           |         |
| (n & (1 << (k - 1))) != 0 |      Check if kth bit is set for a number      |         |
|    n ^ (1 << (k - 1))     |               Toggle the kth bit               |         |

# A. Some cool function
## 1. Loop all sub mask of a mask
```cpp
int n = 4;

for (int mask = 1; mask < (1 << n); mask++) {
	cout << "Current mask " << mask << '\n';
	for (int submask = mask; submask != 0; submask = (submask - 1) & mask) {
		cout << bitset<5>(submask) << '\n';
	}
	cout << '\n';
}
```
## 2. Extracts the last set bit
*Usually use in [[Fenwick Tree]]*
```cpp
int n = 10;
cout << bitset<5>(n) << '\n';  // 01010
n = n & (-n);
cout << bitset<5>(n) << '\n';  // 00010
```
## 3. Sets the last cleared bit
```cpp
int n = 10;
cout << bitset<5>(n) << '\n';  // 01010
n = n | (n + 1);
cout << bitset<5>(n) << '\n';  // 01011
```
## 4. Clears the lowest set bit
```cpp
int n = 10;
cout << bitset<5>(n) << '\n';  // 01010
n = n & (n - 1);
cout << bitset<5>(n) << '\n';  // 01000
```
## 5. Find the last set bit's position
```cpp
int n = 8;
cout << bitset<5>(n) << '\n';  // 01000
cout << log2(n & -n)+1 << '\n';// 4
```
## 6. Clears all trailing ones
```cpp
int n = 15;
cout << bitset<5>(n) << '\n';  // 10111
n = n & (n + 1);
cout << bitset<5>(n) << '\n';  // 10000
```
## 7. Clears all bits from ith bit to right
```cpp
int n = 157;
cout << bitset<10>(n) << '\n';  // 0010011101
int i = 4;
n = n & ((1 << 4) - 1);
cout << bitset<10>(n) << '\n';  // 0000001101
```
## 8. Clears all bits from ith bit to left
```cpp
int n = 157;
cout << bitset<10>(n) << '\n';  // 0010011101
int i = 3;
n = n & ~((1 << i + 1 ) - 1);
cout << bitset<10>(n) << '\n';  // 0010010000
```
## 9. Check if an integer is a power of 2
```cpp
bool isPowerOfTwo(unsigned int n) { 
	return n && !(n & (n - 1)); 
}
```
## 10. Check if the number is divisible by a power of 2
```cpp
bool isDivisibleByPowerOf2(int n, int k) {
    int powerOf2 = 1 << k;
    return (n & (powerOf2 - 1)) == 0;
}
```
## 11. GCC functions
- `__builtin_popcount(unsigned int)` **returns the number of set bits** `__builtin_popcount(0b0001'0010'1100) == 4`

- `__builtin_ffs(int)` **finds the index of the first (most right) set bit** `__builtin_ffs(0b0001'0010'1100) == 3`

- `__builtin_clz(unsigned int)` **the count of leading zeros** `__builtin_clz(0b0001'0010'1100) == 23`

- `__builtin_ctz(unsigned int)` **the count of trailing zeros** `__builtin_ctz(0b0001'0010'1100) == 2`

- `__builtin_parity(x)` **the parity (even or odd) of the number of ones in the bit representation**