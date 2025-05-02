Given an array $A$  of size $N$ . You have to find the minimal non-negative element that is not present in the array. That number is commonly called the **MEX** (minimal excluded).
<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mtable displaystyle="true" columnalign="right left" columnspacing="0em" rowspacing="3pt">
    <mtr>
      <mtd>
        <mtext>mex</mtext>
        <mo stretchy="false">(</mo>
        <mo fence="false" stretchy="false">{</mo>
        <mn>0</mn>
        <mo>,</mo>
        <mn>1</mn>
        <mo>,</mo>
        <mn>2</mn>
        <mo>,</mo>
        <mn>4</mn>
        <mo>,</mo>
        <mn>5</mn>
        <mo fence="false" stretchy="false">}</mo>
        <mo stretchy="false">)</mo>
      </mtd>
      <mtd>
        <mi></mi>
        <mo>=</mo>
        <mn>3</mn>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mtext>mex</mtext>
        <mo stretchy="false">(</mo>
        <mo fence="false" stretchy="false">{</mo>
        <mn>0</mn>
        <mo>,</mo>
        <mn>1</mn>
        <mo>,</mo>
        <mn>2</mn>
        <mo>,</mo>
        <mn>3</mn>
        <mo>,</mo>
        <mn>4</mn>
        <mo fence="false" stretchy="false">}</mo>
        <mo stretchy="false">)</mo>
      </mtd>
      <mtd>
        <mi></mi>
        <mo>=</mo>
        <mn>5</mn>
      </mtd>
    </mtr>
    <mtr>
      <mtd>
        <mtext>mex</mtext>
        <mo stretchy="false">(</mo>
        <mo fence="false" stretchy="false">{</mo>
        <mn>1</mn>
        <mo>,</mo>
        <mn>2</mn>
        <mo>,</mo>
        <mn>3</mn>
        <mo>,</mo>
        <mn>4</mn>
        <mo>,</mo>
        <mn>5</mn>
        <mo fence="false" stretchy="false">}</mo>
        <mo stretchy="false">)</mo>
      </mtd>
      <mtd>
        <mi></mi>
        <mo>=</mo>
        <mn>0</mn>
      </mtd>
    </mtr>
  </mtable>
</math>
# Solution 1: O(nlog(n))
```cpp
int mex(vector<int> const& A) {
    set<int> b(A.begin(), A.end());

    int result = 0;
    while (b.count(result))
        ++result;
    return result;
}
```
# Solution 2: O(n)
```cpp
int mex(const vector<int>& a) {
    int n = (int)a.size();
    vector<int> c(n + 2, 0);
    for (int x : a) {
        if (x <= n + 1) c[x]++;
    }
    int ans = 0;
    while (c[ans]) ans++;
    return ans;
}
```

# With update
https://atcoder.jp/contests/hhkb2020/tasks/hhkb2020_c
https://cp-algorithms.com/sequences/mex.html  solve problem in this
```cpp
struct Mex {
    map<int, int> frequency;
    set<int> missing_numbers;
    vector<int> A;

    Mex(vector<int> const& A) : A(A) {
        for (int i = 0; i <= A.size(); i++)
            missing_numbers.insert(i);

        for (int x : A) {
            ++frequency[x];
            missing_numbers.erase(x);
        }
    }

    int mex() {
        return *missing_numbers.begin();
    }

    void update(int idx, int new_value) {
        if (--frequency[A[idx]] == 0)
            missing_numbers.insert(A[idx]);
        A[idx] = new_value;
        ++frequency[new_value];
        missing_numbers.erase(new_value);
    }
};
```