#tree
**A way to merge two sets efficiently.**
Let's consider a tree rooted at node 111, where each node has a color.

For each node, let's store a set containing only that node, and we want to merge the sets in the nodes subtree together such that each node has a set consisting of all colors in the nodes subtree. Doing this allows us to solve a variety of problems, such as query the number of distinct colors in each subtree.

### Naive Solution

Suppose that we want merge two sets aaa and bbb of sizes nnn and mmm, respectively. One possiblility is the following:

```cpp
for (int x : b) a.insert(x);
```

which runs in $\mathcal{O}(m\log (n+m))$ time, yielding a runtime of $\mathcal{O}(N^2\log N$) in the worst case. If we instead maintain $a$ and $b$  as sorted vectors, we can merge them in $\mathcal{O}(n+m)$ time, but $\mathcal{O}(N^2)$is also too slow.

### Better Solution

With just one additional line of code, we can significantly speed this up.

```cpp
if (a.size() < b.size()) swap(a, b);

for (int x : b) a.insert(x);
```


Note that [swap](http://www.cplusplus.com/reference/utility/swap/) exchanges two sets in $\mathcal{O}(1)$ time. Thus, merging a smaller set of size mmm into the larger one of size nnn takes $\mathcal{O}(m\log n)$ time.

**Claim:** The solution runs in $\mathcal{O}(N\log^2N$) time.