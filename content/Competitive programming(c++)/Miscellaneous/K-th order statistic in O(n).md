Given an array   $A$  of size   $N$  and a number $K$ . The problem is to find $K$ -th largest number in the array, i.e.,   $K$ -th order statistic.
```cpp
    vector<int> a = {1,4,3,5,2};
    int k = 3;
    // O(nlog(n)) solution
    priority_queue<int, vector<int>, greater<int>> pq;
    for (auto i : a) {
        pq.push(i);
    }
    while (pq.size() > k) pq.pop();
    cout << pq.top() << '\n';

    // O(n) solution
    auto it = a.end() - k;
    nth_element(a.begin(), it, a.end()); 
    cout << *it;
```