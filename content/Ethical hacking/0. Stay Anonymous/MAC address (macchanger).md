# MAC (Media Access Control)
- Is a physical address assigned to each network *interface card (NIC)* on the device.
- You can understand:
	 - *IP Address* = identifies the "location" in the network.
	 - *MAC Address* = identifies the real "machine".
- MAC is in the form of **6 groups of 2 hex characters** (numbers 0-9, letters A-F), **separated by : or -**.
Example: **00:0C:29:3C:4D:5E**
- First 3 groups (**00:0C:29**) = **Manufacturer code** (eg: Apple, Intel, Cisco...), *you can copy first 3 group then google to see*.
- The following 3 groups (**3C:4D:5E**) = Unique device code.
# macchanger (macchanger -h)
![[Screenshot 2025-04-27 164904.png]]
- **Permanent MAC** is your current *network interface card* or *your network card* and *not belong to computer*.

![[Screenshot 2025-04-27 170801.png]]
<center><strong>Run `macchanger -a eth0` to change your MAC address</strong></center>

![[Screenshot 2025-04-27 171244.png]]
<center><strong>Run `macchanger -p eth0` to reset MAC address</strong></center>
