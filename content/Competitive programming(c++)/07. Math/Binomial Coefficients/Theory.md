# 1. Factorials
A factorial is the product of all positive integers less than or equal to a given positive integer. In other words $n! = n × (n - 1) × (n - 2) × · · · × 1$.
# 2. Permutations Basics
The number of ways of **arranging n objects in a line** is: $n!$

The number of ways of **arranging n objects in a circle** where rotations of the same arrangement **aren’t** considered **distinct** is $(n - 1)!$

The number of ways of **arranging n objects in a circle** where rotations of the same arrangement **aren’t** considered **distinct** and **reflections** of the same arrangement aren’t considered distinct is $\frac{(n - 1)!}{2}$

The number of ways to **arrange k objects out of n** total objects is
$${}^nP_k = \frac{n!}{(n-k)!}$$
**Example**:
>Gauss, Pauli, Einstein, Newton, Edison, and Faraday are sitting at a circular table. Einstein, Newton, and Faraday are enemies so they refuse to sit next to each other. With this condition, how many ways are there for the 6 physicists to sit at the table if rotations are not counted as distinct orientations?

Without the condition, the answer would just be $(6 - 1)! = 5! = 120$ as we can fix 1 of the 6 people at the top and permute the remaining 5 people.

**Where must the enemies sit so that none of them are sitting next to each other?**

Since there are only 6 total people, there must be exactly one other person between each of the enemies as seen in the diagram below where the red X’s represent the enemies.

**Should we again try to fix someone at the top to deal with rotations?**

Yes! By fixing Gauss at the top (arbitrarily), we can now simply order the remaining 5 people without having to worry about the rotation condition.

**How many ways to permute the enemies?**

Note that the 3 possible locations of the enemies is fixed so there are 3! ways to permute them.

**How many ways to permute the people with no enemies?**

We already fixed Gauss (someone without enemies) so there are 2! ways to permute the other 2 people who don’t have enemies.

In total, our answer is $3! × 2! = 12$

# 3. Word Rearrangements
**Theorem: The number of ways to order a word is**
$$\frac{n!}{d1! × d2! × d3! × . . .}$$
where $n$ is the number of letters and $d1, d2, d3, . . .$ are the number of times each of the letters that occur more than 1 time appear in the word.

**Example**
In a shooting match, eight clay targets are arranged in two hanging columns of three targets each and one column of two targets. A marksman is to break all the targets according to the following rules:

1) The marksman first chooses a column from which a target is to be broken.

2) The marksman must then break the lowest remaining target in the chosen column.

If the rules are followed, in how many different orders can the eight targets be broken?
![[Pasted image 20241010092443.png]]

Clearly, the marksman must shoot the left column three times, the middle column two times, and the right column three times.
From left to right, suppose that the columns are labeled $L$, $M$, $R$. We consider the string $LLLMMRRR$
Since the letter arrangements of $LLLMMRRR$ and the shooting orders have one-to-one correspondence, we count the letter arrangements:
$$\frac{8!}{3!.2!.3!}$$
# 4. Combinations
**Theorem (Combinations Formula)**  
The number of ways to choose $k$ objects out of a total of $n$ objects is
$$\binom{n}{k} = \frac{n!}{k!(n-k)!} = \frac{n(n-1) \dots (n-k+1)}{k!}$$
This is typically spoken as "n choose k".
**Pascal's Triangle:** $$\binom n k = \binom {n-1} {k-1} + \binom {n-1} k$$
**Symmetry rule:** $$\binom n k = \binom n {n-k}$$
Keeps the lower index intact $$(n - k)\binom n k = n\binom {n-1} {k}$$
Factoring in: $$\binom n k = \frac n k \binom {n-1} {k-1}$$
Sum over   $k$: $$\sum_{k = 0}^n \binom n k = 2 ^ n$$
Sum over $n$: $$\sum_{m = 0}^n \binom m k = \binom {n + 1} {k + 1}$$
Sum over $n$ and $k$: $$\sum_{k = 0}^m  \binom {n + k} k = \binom {n + m + 1} m$$
Summation on the upper index: $$\sum_{k = 0}^m  \binom k n = \binom {m + 1} {n + 1}$$
Sum of the squares: $${\binom n 0}^2 + {\binom n 1}^2 + \cdots + {\binom n n}^2 = \binom {2n} n$$
Weighted sum: $$1 \binom n 1 + 2 \binom n 2 + \cdots + n \binom n n = n 2^{n-1}$$
