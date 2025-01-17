# 03 - Whonix

## 3.1 - Hypervisor Type 2

### 3.1.1 - VirtualBox

Select the virtual machine and click on **Settings**

![[01 - VM VirtualBox Manager Settings.png]]

Change **NAT** to **Internal Network**

![[02 - VM VirtualBox Manager Network.png]]

You're all set user!

![[03 - VM VirtualBox Manager Adapter.png]]

### 3.1.2 - Network Settings

Depending on your **DE (Desktop Environment)** on GNU/Linux search for anything related to **Network Configuration Settings.**

![[01 - Advanced Network Configuration XFCE.png]]

Click the **plus** icon.

![[02 - Network Connections XFCE.png]]

Choose **Ethernet**.

![[03 - Network Connection Type XFCE.png]]

Go to IPv4 Settings and enter the following details from this table below.

|Address|Netmask|Gateway|DNS servers|
|---|---|---|---|
|10.152.152.2-254|255.255.192.0 or 18|10.152.152.10|10.152.152.10|

---
## References

- [Check TOR](https://check.torproject.org/)

- [Ifconfig.me](https://ifconfig.me/)

- [IP.me](https://ip.me/)

- [IPInfo](https://ipinfo.io)

- [Amazon AWS IP Lookup](https://checkip.amazonaws.com)

- [Subnet Cheatsheet Subnet Mask](https://www.freecodecamp.org/news/subnet-cheat-sheet-24-subnet-mask-30-26-27-29-and-other-ip-address-cidr-network-references/)