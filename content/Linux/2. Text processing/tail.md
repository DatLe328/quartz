Example we want to get last 10 line of `/etc/sudoers` file just type
```bash
tail /etc/sudoers
```
> Add `nl` command to see more clearly

If we want to adjust the number of lines then use `-n`
```bash
tail -n 20 /etc/sudoers
```
This command will display last 20 lines of file

But what if we want to display from line 40 to the end 
of the file, just use `-n +40`
```bash
tail -n +40 /etc/sudoers
```