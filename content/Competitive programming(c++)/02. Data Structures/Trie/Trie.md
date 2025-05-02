```cpp
/*
	https://cses.fi/problemset/task/1731/

	Note:
		- This data structure support insert, delete, find for string
		- If you want to use number instead of string go for TrieBinary.cpp
*/
struct Trie {
	struct Node {
		vector<int> child;
		int cnt, exist;
		Node() : child(26, -1), cnt(0), exist(0) {}
	};
	vector<Node> nodes;
	Trie() :  nodes(1) {}
	void add_string(string s) {
		int v = 0;
		int n = int(s.size());
		for (int i = 0; i < n; i++) {
			int c = int(s[i] - 'a');
			if (nodes[v].child[c] == -1) {
				nodes[v].child[c] = nodes.size();
				nodes.emplace_back();
			}
			v = nodes[v].child[c];
			nodes[v].cnt++;
		}
		nodes[v].exist++;
	}

    void delete_string(string s) {
        if (find_string(s) == false) return;
        int pos = 0;
		int n = int(s.size());
        for (int i = 0; i < n; i++) {
			int c = int(s[i] - 'a');

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


    bool find_string(string s) {
        int pos = 0;
        for (auto f : s) {
            int c = f - 'a';
            if (nodes[pos].child[c] == -1) return false;
            pos = nodes[pos].child[c];
        }
        return (nodes[pos].exist != 0);
    }
};

```