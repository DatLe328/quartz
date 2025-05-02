```cpp
void euler_tour(int u, int p) {
    num[u] = tail[u] = ++timer;
    for (int v : adj[u]) {
        if (v == p) continue;
        if (!num[v]) {
            euler_tour(v, u);
        }
    }
    tail[u] = timer;
}

```
- Dùng để xác định các node nào sẽ là subnode của node nào [[Check in sub tree]]
- Ngoài ra có thể kết hợp với các cấu trúc dữ liệu khác để thực hiện các truy vấn như tính tổng các subtree, cập nhật, tìm min/max, gcd, ...

# I. Euler paths and Euler Cycles
## A. Undirected Graph
**Theorem:** Let G be an undirected, connected graph. Then G will have an Euler cycle when
and only if every vertex of G has even degree. G will have a path but no cycle
Euler program if and only if G has exactly two vertices of odd degree

**Code: [[Mail Delivery|Euler Cycle]]**

![[euler_path_undirected.png]]
Every vertex of the graph G in the figure above has even degree, so G has an Euler cycle.
## B. Directed Graph
Let G be a directed and weakly connected graph. Then G will have an Euler cycle if and only if G is balanced (every vertex of G has an output degree equal to an input degree). G will have a path but no Euler cycle if and only if G has exactly two vertices of odd degree x and y satisfying:
$$\left\{ \begin{aligned} d^{+}(x)= d^{-}(x) + 1\\ d^{-}(y)= d^{+}(y)+1 \end{aligned} \right.$$

 **Code: [[Teleporters Path|Euler Path]]**

**Example:** Considering the directed, connected graph G in the figure below, we have:
![[euler_path_directed.png]]
$$ \begin{aligned} &d^+(a) = 2, d^-(a) = 1 \implies d^+(a) = d^-(a) + 1 \\ &d^+(d) = 2, d^-(d) = 3 \implies d^-(d) = d^+(d) + 1 \\ &d^+(b) = d^-(b) = 2 \\ &d^+(c) = d^-(c) = 2 \end{aligned} $$
Because G has exactly two vertices of odd degree a and d that satisfy the above condition, G has a path but there is no Euler cycle, moreover that path starts at a and ends at d:
$$ a \to b \to d \to c \to a \to d \to b \to c \to d $$
