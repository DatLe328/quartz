```cpp
struct Trie {
    const int LG = 31;
	struct Node {
		vector<int> child;
		int cnt, exist;
		Node() : child(2, -1), cnt(0), exist(0) {}
	};
	vector<Node> nodes;
	Trie() :  nodes(1) {}
	void add_number(int x) {
		int v = 0;
		for (int i = LG; i >= 0; i--) {
            int c = (x >> i) & 1;
			if (nodes[v].child[c] == -1) {
				nodes[v].child[c] = nodes.size();
				nodes.emplace_back();
			}
			v = nodes[v].child[c];
			nodes[v].cnt++;
		}
		nodes[v].exist++;
	}
    void delete_number(int x) {
        if (find_number(x) == false) return;
        int pos = 0;
        for (int i = LG; i >= 0; i--) {
            int c = (x >> i) & 1;

            int tmp = nodes[pos].child[c];
            nodes[tmp].cnt--;
            if (nodes[tmp].cnt == 0) {
                nodes[pos].child[c] = -1;
                return;
            }

            pos = tmp;
        }
        nodes[pos].exist--;
    }

    bool find_number(int x) {
        int pos = 0;
        for (int i = LG; i >= 0; i--) {
            int c = (x & (1 << i) ? 1 : 0);
            if (nodes[pos].child[c] == -1) return false;
            pos = nodes[pos].child[c];
        }
        return (nodes[pos].exist != 0);
    }
};

```