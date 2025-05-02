## **Step 1:** Install **tor** service
```bash
sudo apt update
sudo apt upgrade -y
sudo apt install tor

service tor status # check for tor.service status
service tor start  # start tor service if it hasn't started
```
## **Step 2:** Edit `proxychains4.conf` file
```bash
sudo vim /etc/proxychains4.conf

# Then uncomment this line
dynamic_chain

# Then comment this line and save file
strict_chain
```
## **Step 3:** Results
> Run this command in normal user mode
```bash
proxychains firefox www.duckduckgo.com
```
> Then search for `dns leak test` and check the result if your IP has changed

![[Screenshot 2025-04-27 163045.png]]
<center><strong>The location has changed from Vietnam to Netherlands</strong></center>
