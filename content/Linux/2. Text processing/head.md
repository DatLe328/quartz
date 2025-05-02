Example we want to get first 10 line of `/etc/sudoers` file just type
```bash
head /etc/sudoers
```
> Add `nl` command to see more clearly

If we want to adjust the number of lines then use `-n`
```bash
head -n 20 /etc/sudoers
```
This command will display first 20 lines of file