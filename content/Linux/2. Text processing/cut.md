Look at this example
```bash
ping 192.168.1.1 -c 1 > ip.txt
```
This is the content of `ip.txt` file:
```text
PING 192.168.1.1 (192.168.1.1) 56(84) bytes of data.
64 bytes from 192.168.1.1: icmp_seq=1 ttl=128 time=3.51 ms
```
*We want to get only the IP address, how can we do it*
First use `grep` to find `64` and get only the second line:
`cat ip.txt | grep 64`
Now we have
`64 bytes from 192.168.1.1: icmp_seq=1 ttl=128 time=3.51 ms`

But we haven't done yet. We want to separate every field by space, now we need to use `cut` command to get the 4th field:
```bash
cat ip.txt | grep 64 | cut -d ' ' -f 4
```
- `-d` is delimiter (default is TAB)
- `-f` is field
Now we have:
```bash
192.168.1.1:
```
But there's still a little colon on the end now we need to use `tr`(translate) to get rid of this
```bash
cat ip.txt | grep 64 | cut -d ' ' -f 4 | tr -d ':'
```
There you go an IP address
```text
192.168.1.1
```