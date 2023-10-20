# Havoc

Search Tag: #red-team-infrastructure

## 01 - Setup

### 1.1 - Firewall Rules

```
# mkdir /etc/iptables
# iptables -I INPUT 1 -p tcp -s 0.0.0.0/0 --dport 40056 -j DROP
# iptables -I INPUT 1 -p tcp -s 127.0.0.1 --dport 40056 -j ACCEPT
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

### 1.2 - Domain Fronting

TODO: Provide an example usage for Havoc C2

## 02 -Detection

`ssl:postalCode=3540 ssl.jarm:"3fd21b20d00000021c43d21b21b43de0a012c76cf078b8d06f4620c2286f5e"`