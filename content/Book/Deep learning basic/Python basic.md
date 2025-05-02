# 1. Data structure
## 1.1 Dictionary
```python
	dict = {'a': 1}    # Or dict = dict()
	dict['a'] = 2      # Update value or assign
	print(dict['b'])      # Error
	print(dict.get('b'))  # None
	
	del dict['b']    # Error
	dict.pop('b')    # Best way to remove
	
	# Print key
	for key in dict:
	    print(key)    # a
	# Print value
	for value in dict.values():
	    print(value)  # 2
	# Print key and value
	for key, value in dict.items():
	    print(key, value)    # a 2
```
## 1.2 Set
```python
	st = set()    # Or st = {None}  Warning!
	print(type({None}))    # <class 'set'>
	print(type({}))        # <class 'dict'>
	
	st.add(2)
	st.remove(3)    # Error: 3 not exist
	st.discard(3)   # Safe remove element
	
	st.pop()    # Remove first element
```
## 1.3 Stack and queue
```python
	from collections import deque
	
	stack = deque()
	queue = deque()
	
	# Add
	stack.append(2)
	queue.append(2)
	# Pop
	stack.pop()
	queue.popleft()
	# Get value
	print(stack[-1])
	print(queue[0])
```
## 1.4 Priority queue
```python
def dijkstra(graph, start):
    INF = float('inf')
    dist = {node: INF for node in graph}
    dist[start] = 0
    heap = [(0, start)]
    
    while heap:
        d, u = heapq.heappop(heap)
        if d > dist[u]:
            continue

        for v, w in graph[u]:
            if dist[u] + w < dist[v]:  
                dist[v] = dist[u] + w
                heapq.heappush(heap, (dist[v], v))  
    
    return dist
```
## 1.5 Tuple
```python
	my_tuple = tuple()   # Or my_tuple = ()
	my_tuple = (2, 3)
	x, y = my_tuple
	print(my_tuple)      # (2, 3)
	print(x, y)          # 2 3
	print(my_tuple[1])   # 3
```

# 2. Controlling the flow
## 2.1 Conditional expression
```python
	x = <true_value> if <condition> else <false_value>
	
	val = 22
	str = "odd" if val % 2 else "even"
	print(str)     # even
```
## 2.2 For loops
```python
	for i, char in enumerate("abc"):
		print(i, char)
	# 0 a
	# 1 b
	# 2 c
	
	range(start, stop, step)
	
	evens_number = range(0, 10, 2)   # evens number from [0, 9]
	
	
	# Zip
	courses = [1, 2, 3]
	ranks = ["good", "better", "best!"]
	zipped = zip(courses, ranks)
	for course, rank in zipped:
	    print(course, rank)
	# 1 good
	# 2 better
	# 3 best!
```
## 2.3 Iterator
### 2.3.1 Loop and map
```python
	arr = [1,2,3,4]
	for i in iter(arr, ::2):
		print(i)    # 1 3

	for i in map(str, i):
		print(i)    # ['1', '2', '3', '4']
```
### 2.3.2 Change data type
```python
	arr = [1,2,3,4]
	it = iter(arr)
	new_arr = list(map(str, it))
	print(new_arr)    # ['1', '2', '3', '4']
```
# 3 Error handling
## 3.1 Try-Except
```python
	for i in range(-3, 3):
	    try:
	        print(10 / i)
	    except ZeroDivisionError:
	        print("Can't devide by zero!")
```
# 4. Reading and writing files
```python
	file = open("A_input.txt", "r")
	for line in file:
	    print(line)
	file.close()
	
	# Must use with open if you want to use file.read()
	with open("firstname.txt", "r") as f:
	    print(f.read())
	    f.close()
	
	# Write file
	data = [4,3,6,8]
	with open("A_input.txt", "w") as f:
	    for i in data:
	        f.write(str(i) + '\n')
	    f.close()
	
	# Append data into file
	data = [4,3,6,8]
	with open("A_input.txt", "a") as f:
	    for i in data:
	        f.write(str(i) + '\n')
	    f.close()
```
# 5. Packaging and reusing code
## 5.1 Functions
```python
	def calc(x, *args):
	    total = 0
	    for n, a in enumerate(args):
	        total += a * (x**n)
	        print(total)
	    return total
	# ax^0 + ax^1 + ax^2 +....
	print(calc(10, 1, 2, 3))


	def calc(x,  **kwargs):
	    total = 0
	    for n, a in kwargs.items():
	        print(n, a)
	    return total

	print(calc(10, _0=1, _1=3, _2=5))
	# _0 1
	# _1 3
	# _2 5
```
## 5.2 Lambda
```python
	add = lambda x, y: x + y 
	print(add(3, 7))

	pairs = [(1, 3), (2, 2), (4, 1)] 
	pairs.sort(key=lambda x: x[1])
```
## 5.3 Comprehensions
### 5.3.1 List comprehension
```python
	[<expression> for <variables> in <iterable> if <condition>]
	
	[i**2 for i in range(5)] # [0, 1, 4, 9, 16]


	squares = (i**2 for i in range(1, 4))
	for s in squares:
	print(s)  # 1 4 9
```
### 5.3.2 Dictionary comprehension
```python
	{<key>:<value> for <variables> in <iterable> if <condition>}
	
	{i:i**2 for i in range(4)}
	# {0:0, 1:1, 2:4, 3:9}
```

# 6. Numpy
## 6.1 Array
### 6.1.1 Create array
```python
arr = np.array(1)    # 0-D array
print(arr)    # 1

arr = np.array([1,2,3])  # 1-D array
print(arr[0])    # 1

arr = np.array([     # 2-D array
	[1,2],
	[3,4]
])
print(arr[1, 1])   # 4

arr = np.array([[[1, 2, 3], [4, 5, 6]], [[1, 2, 3], [4, 5, 6]]])   # 3-D array
print(arr[0,0,1])   # 2
```
### 6.1.2 Insert, delete element
```python
arr = np.array([1, 2, 3])
new_arr = np.append(arr, 4)   # Push back
new_arr = np.append(arr, 1, 99)  # 1 99 2 3
new_arr = np.delete(arr, 1)   # Remove number 99
new_arr = np.delete(arr, [1, 2])  # Remove number 2 3

```
### 6.1.3 Higher dimensional array
```python
arr = np.array([1, 2, 3, 4], ndmin=5)

print(arr)
print('number of dimensions :', arr.ndim
```
### 6.1.4 Shape and reshape
The shape of an array is the number of elements in each dimension.
```python
arr = np.array([[1, 2, 3, 4], [5, 6, 7, 8]])  
  
print(arr.shape)    # (2, 4)
```
- By reshaping we can add or remove dimensions or change number of elements in each dimension.
- The shape and the size must equal
```python
arr = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12])
newarr = arr.reshape(4, 3)
print(newarr)
```
If we don't know how many dimension we need just add `-1`
```python
arr = np.array([[1, 2, 3, 4],[5, 6, 7, 8]])
newarr = arr.reshape(2, -1)
print(newarr)
```
## 6.1.5 Column stack
Given a1=[1,-2,-5] and a2=[2,5,6]. Create a matrix M such that the first column is a1 and the second column is a2
```python
a1 = np.array([1 ,-2 ,-5])
a2 = np.array([2 ,5 ,6])
m = np.column_stack((a1, a2))

```
## 6.2 Data type
- `i` - integer
- `b` - boolean
- `u` - unsigned integer
- `f` - float
- `c` - complex float
- `m` - timedelta
- `M` - datetime
- `O` - object
- `S` - string
- `U` - unicode string
```python
print(arr.dtype)    # Print data type

newarr = arr.astype(int)   # Change data type
```
## 6.3 Copy and view
Make a copy doesn't change the original array
```python
arr = np.array([1, 2, 3, 4, 5])  
x = arr.copy()  
arr[0] = 42  
  
print(arr)     # 1 2 3 4 5
print(x)       # 42 2 3 4 5
```
Make a view can change the original array
```python
arr = np.array([1, 2, 3, 4, 5])  
x = arr.view()  
arr[0] = 42  
  
print(arr)     # 42 1 2 3 4 5
print(x)       # 42 1 2 3 4 5
```
## 6.4 Iterating
```python
arr = np.array([[1, 2, 3, 4], [5, 6, 7, 8]])  
  
for x in np.nditer(arr[:, ::2]):  
	print(x)

for idx, x in np.ndenumerate(arr):  
  print(idx, x)
```