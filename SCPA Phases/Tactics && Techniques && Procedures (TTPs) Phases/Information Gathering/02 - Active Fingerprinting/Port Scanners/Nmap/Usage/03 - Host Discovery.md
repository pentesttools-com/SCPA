# 03 - Host Discovery

## 3.1 - IP Range and List

- Range of IP targets to scan

`$ sudo nmap 192.168.0.23-110`

- List of IP targets to scan

`$ sudo nmap 192.168.0.23,110,166`

- List of IP targets in a text file

`$ sudo nmap -iL targets.txt`

- Nmap can check which hosts to scan without scanning them

`$ sudo nmap -sL <IP>/<CIDR>`

## 3.2 - ICMP Scan

- Ping Sweep

`$ sudo nmap -sn <IP>/<CIDR>`

`$ nmap -sn -T4 <IP>/<CIDR> -oG - | awk '/Up$/{print $2}'`

- Echo Scan

`$ sudo nmap -PE -sn <IP>/<CIDR>`

- Timestamp Scan

`$ sudo nmap -PP -sn <IP>/<CIDR>`

- Address Mask Scan

`$ sudo nmap -PM -sn <IP>/<CIDR>`

## 3.3 - ARP Scan

`$ sudo nmap -PR -sn <IP>/<CIDR>`

## 3.4 - TCP Scan

- TCP SYN Ping Scan

`$ sudo nmap -sn -PS22,80,443 <IP>/<CIDR>`

- TCP ACK Ping Scan

`$ sudo nmap -sn -PA22,80,443 <IP>/<CIDR>`

## 3.5 - UDP Scan

`$ sudo nmap -sn -PU53,88,161,162 <IP>/<CIDR>`

## 3.6 - DNS Lookup

- No Reverse-DNS Lookup

`$ sudo nmap -n <IP>/<CIDR>`

- Reverse-DNS lookup

`$ sudo nmap -R <IP>/<CIDR>`

If you want to specify a specific dns server to lookup

`$ sudo nmap --dns-servers <dns_server_1>,<dns_server_2>,<dns_server_n> <IP>/<CIDR>`

## 3.7 - Traceroute

`$ sudo nmap --traceroute -sn -PE <IP>`

## 3.8 - Resolve IP Addresses

TODO: Test this NSE script

```
$ nmap --resolve-all <IP>

$ nmap --script resolveall --script-args=newtargets,resolveall.hosts={<host1>, ...} <IP>
```

## 3.9 - ICMP Forwarding Enabled

`$ sudo nmap -sn --script ip-forwarding --script-args='target=<website.com>' <IP>`

---
## References

- [Nmap usage tips](https://miloserdov.org/?p=3639)

- [Nmap NSEDocs: Script resolveall](https://nmap.org/nsedoc/scripts/resolveall.html)