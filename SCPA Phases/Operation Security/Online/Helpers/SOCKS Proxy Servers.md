# SOCKS Proxy Servers

## 01 - Proxychains

`$ cat socks5.conf`

---

```
strict_chain
proxy_dns
tcp_read_time_out 15000
tcp_connect_time_out 8000
localnet 127.0.0.1/255.255.255.255
[ProxyList]
socks4  <IP> <PORT>
socks5  <IP> <PORT> [username] [password]
http    <IP> <PORT> [username] [password]
```

Refer to Proxychains under Pivoting SOCKS proxy section

Refer to [[Basic|ping sweep one liners]] to grab active proxy servers then pass it to the script that will format for proxychains

```bash
#!/bin/bash

# Name it as proxychains-formatter.sh

PROXY=$1
FILE=$2
PORT=$3
USERNAME=$4
PASSWORD=$5

function usage() {
    echo "Usage: $0 <SOCKS4 | SOCKS5 | HTTP> ips.txt <PORT> <username> <password>"
}

function format() {
    while read -r IP
    do
        if [ ${PROXY,,} = "socks4" ]
        then
                echo "socks4 ${IP} ${PORT}"
        elif [ ${PROXY,,} = "socks5" ]
        then
                echo "socks5 ${IP} ${PORT} ${USERNAME} ${PASSWORD}"
        elif [ ${PROXY,,} = "http" ]
        then
                echo "http ${IP} ${PORT} ${USERNAME} ${PASSWORD}"
            fi
    done < $FILE
}

if [ $# -lt 5 ] || [ $PROXY = "-h" ]
then
       usage
       exit 1
fi

format
```

## 02 - Privoxy

`$ cat /etc/privoxy/config`

---

```
user-manual /usr/share/doc/privoxy/user-manual/
confdir /etc/privoxy
logdir /var/log/privoxy
actionsfile match-all.action # Actions that are applied to all sites and maybe overruled later on.
actionsfile default.action   # Main actions file
actionsfile user.action      # User customizations
#actionsfile regression-tests.action     # Tests for privoxy-regression-test
filterfile default.filter
filterfile user.filter      # User customizations
logfile logfile
listen-address  127.0.0.1:8118
enable-remote-toggle  0
enable-remote-http-toggle  0
enable-edit-actions 0
enforce-blocks 0
buffer-limit 4096
enable-proxy-authentication-forwarding 0
forwarded-connect-retries  0
accept-intercepted-requests 0
allow-cgi-request-crunching 0
split-large-forms 0
keep-alive-timeout 5
tolerate-pipelining 1
socket-timeout 300
forward-socks5  /   <username>:<password>@<IP>:<PORT>  .
```

`$ privoxy /etc/privoxy/config`

Refer to [[Terminal Setup]] section to configure a bash resource file

```
$ proxy_on
username:
server: localhost
port: 8118
```

`$ curl https://ipinfo.io`

Refer to [[Basic|ping sweep one liners]] to grab active proxy servers then pass it to the script that will format for privoxy

```bash
#!/bin/bash
PROXY=$1
FILE=$2
PORT=$3
USERNAME=$4
PASSWORD=$5

# Name it as privoxy-formatter.sh

function usage() {
    echo "Usage: $0 <SOCKS4 | SOCKS5 | HTTP> ips.txt <PORT> <username> <password>"
}

function format() {
    while read -r IP
    do
            if [ ${PROXY,,} = "socks4" ]
            then
                    echo "forward-socks4a   /       ${IP}:${PORT}   ."
            elif [ ${PROXY,,} = "socks5" ]
            then
                    echo "forward-socks5    /       ${USERNAME}:${PASSWORD}@${IP}:${PORT}   ."
            fi
    done < $FILE
}

if [ $# -lt 5 ] || [ $PROXY = "-h" ]
then
        usage
        exit 1
fi

format
```