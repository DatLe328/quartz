## Step 1: Check your current **DNS server**
```bash
cat /etc/resolv.conf
```
![[Pasted image 20250427174329.png]]
<center><strong>Command display</strong></center>


## Step 2: NetworkManager (opt)
- Install `network-manager` package
```bash
sudo apt update
sudo apt install network-manager
```
- Then enable it
```bash
sudo systemctl start NetworkManager
```
- For automatically start up
```shell
sudo systemctl enable NetworkManager
```
- Check status
```bash
sudo systemctl status NetworkManager
```
## Step 3: Edit DHCP config
```bash
sudo vim /etc/dhcp/dhclient.conf
```
- Then uncoment this line
```bash
prepend domain-name-server 127.0.0.1;
```
- Go to https://umbrella.cisco.com/ to get free DNS at the end of page, then we have something like this:
```bash
prepend domain-name-server 208.67.222.222, 208.67.220.220;
```
## Step 4: Restart Service
- Restart Network service
- Method 1:
```bash
sudo dhclient -r
sudo dhclient
```
- Method 2:
```bash
sudo systemctl restart NetworkManager
```
- Then run `cat /etc/resolv.conf` to check for update

![[Pasted image 20250427181531.png]]
<center><strong>After restart service</strong></center>