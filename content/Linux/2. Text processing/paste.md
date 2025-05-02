Allows users to combine data by columns rather than rows
Example we have `text1.txt` file:
```text
cat
dog
tiger
elephant
```
And `text2.txt` file:
```text
1
2
3
```
Now we use `paste` command:
```bash
paste text1 text2
```
Then we have:
```text
cat    1
dog    2
tiger  3
elephant
```