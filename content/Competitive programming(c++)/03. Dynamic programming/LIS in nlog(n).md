```cpp
// https://www.geeksforgeeks.org/problems/increasing-sub-sequence1712/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=bottom_sticky_on_article
    int maxLength(string s)
    {
        // code here
        vector<char> res;
        res.push_back(s[0]);
        for (int i = 1; i < s.size(); i++) {
            if (s[i] > res.back()) {
                res.push_back(s[i]);
            }
            else {
                auto it = lower_bound(res.begin(), res.end(), s[i]) - res.begin();
                res[it] = s[i];
            }
        }
        return res.size();
    }
```