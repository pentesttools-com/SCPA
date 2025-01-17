# Checklist

Search Tag(s): #checklist #sql-injection #webapp

TODO: Fill the rest of checklist for SQL injection

## Information Gathering

### Passive Reconnaissance

#### Discover Hidden Parameters

- [ ] Find common parameters to perform SQL injection via [[Tactics && Techniques && Procedures (TTPs) Phases/Information Gathering/01 - Passive Fingerprinting/OSINT/Search Engines Dorking/Google Dorking/Dorks/Web Enumeration/SQL Injection/Manual|google dorks]].
- [ ] Spider the webservers with [[Katana|katana]] or [[Gospider|gospider]] to find hidden parameters.
- [ ] Browse the website with OWASP ZAP or Burp Suite to see any potential interest.

### Active Reconnaissance

#### Active Enumeration

##### Discover Ports

- [ ] Probing ports to check what service version of the webservers using [[06 - Scan Techniques|nmap]].

##### Fuzz Hidden Parameters

TODO: I got this covered - Userware

- [ ] To bruteforce hidden parameters use [[Tactics && Techniques && Procedures (TTPs) Phases/Information Gathering/02 - Active Fingerprinting/Enumeration/Fuzzers/Bruteforce Parameters/FFuF|Fuff]] or [[Tactics && Techniques && Procedures (TTPs) Phases/Information Gathering/02 - Active Fingerprinting/Enumeration/Fuzzers/Bruteforce Parameters/Wfuzz|Wfuzz]].
- [ ] [[Arjun]] and [[x8]] and [[WaybackURLs]] and [[Tactics && Techniques && Procedures (TTPs) Phases/Information Gathering/01 - Passive Fingerprinting/Enumeration/HTTP/Wayback Machine/Metasploit|Metasploit]].

## Vulnerability Assessment

### Scanners

#### General

- [ ] Use [[Wapiti]] to find potential SQL injection exploits.
- [ ] Run [[Tactics && Techniques && Procedures (TTPs) Phases/Vulnerability Assessment/Web Vulnerability Scanner/Nikto|Nikto]] scanner to discover for existing SQL injection exploits.
- [ ] You can use XSS scanners to perform and inject Cross-Site Scripting (XSS) payloads for heuristic checks as a probable indicator for SQL injection exploits.
	- [[Dalfox]]
	- [[Tactics && Techniques && Procedures (TTPs) Phases/WebApp Hacking/Injection/Cross Site Scripting (XSS)/Scanners/XSSer|XSSer]]
	- [[XSStrike]]
- [ ] Run an [[Tactics && Techniques && Procedures (TTPs) Phases/WebApp Hacking/Injection/SQL Injection/01 - In-Band SQL Injection/Scanners/Nmap|NSE script]] to probe for SQL injection vulnerabilities on the webservers with `-iL ips.txt` flag.
- [ ] Using [[Tactics && Techniques && Procedures (TTPs) Phases/Vulnerability Assessment/Vulnerability Scanner/Nuclei]] with fuzzer templates to discover GET request URLs with SQL injection vulnerabilities.

#### Content Management System

- [ ] Run [[WPScan]] to check for vulnerable wordpress plugins or themes related to SQL injection.
- [ ] Run [[Joomscan]] to check for vulnerable joomla plugins or themes related to SQL injection.

## Exploit

### Manual Injection on Parameters

- Refer to [[In-Band SQL Queries]] to aid to exploit the vulnerability.

#### Common Parameters

- [ ] Check the top parameters in [[Tactics && Techniques && Procedures (TTPs) Phases/Information Gathering/01 - Passive Fingerprinting/OSINT/Search Engines Dorking/Google Dorking/Dorks/Web Enumeration/SQL Injection/Manual|google dorks]].

#### GET Request

- [ ] Check the URL parameters.
- [ ] Check the URI parameters.
- [ ] Perform on numeric parameters with mathematical expression as a probable indicator for SQL injection. In **SQLMap** this can be possible with `--invalid-bignum` flag.

```
--invalid-logical   Use logical operations for invalidating values
--invalid-string    Use random strings for invalidating values
```

#### POST Request

- [ ] Perform username enumeration to craft the SQL injection
- [ ] Check JSON parameters
- [ ] POST parameters may contain in SOAP/XML

### Manual Injection on Headers

- [ ] Inject **X-Forwarded-For**
- [ ] Inject **X-Forwarded-Host**
- [ ] Inject Cross-Site Scripting (XSS) payloads as a probable indicator of SQL injection.

### SQL Injection Responses

TODO: Fill out the page comparison for SQL injection

##### Boolean-Based SQL Injection

##### Error-Based SQL Injection

##### Union-Based SQL Injection


### Data Collection

#### Login Panels

- [ ] Perform Boolean-based SQL injection to bypass authentication. In **SQLMap** you can specify `--technique=B` flag which allows you to dump the database credentials. Whatever based on your enumeration gathering based on the webapp response include these flags to narrow it down: 
	-  `--string` for single response or `--regexp` multiple responses if returned true. `--code=200` if HTTP code status is true or `--code=401` if HTTP code status is false.
	- `--not-string` for single response if returned false.
	- To perform another check to highest success rate of SQL injection exploit add `--titles` flag to compare the webpages based on title response.
	- if possible you can use other two in-band SQL injection techniques:
		- [ ] Error-based query (`--technique=E`).
		- [ ] UNION query (`--technique=U`). Otherwise, stick with boolean-based technique. To narrow it down:
			- `--union-cols` to specify number of fields.
			- `--union-from` to enforce UNION query technique. in some DBMS(es) instances such as, **Microsoft Access** requires table to validate the response.
- [ ] Find an login (admin) panel to login the credentials you've obtained from the current database. Use [[Gobuster]] if you must.

#### DBMS Credentials

- [ ] After you successfully exploited the parameters. Extract the DBMS credentials to authenticate the database server. You can perform this in [[Tactics && Techniques && Procedures (TTPs) Phases/WebApp Hacking/Injection/SQL Injection/03 - Automated Exploitation/Scenarios/Discovery and Enumeration/Database/SQLMap/05 - Credentials|SQLMap]].
- [ ] Run a port scanner like [[Tactics && Techniques && Procedures (TTPs) Phases/Information Gathering/Active Fingerprinting/Port Scanners/Nmap/Usage/Nmap|Nmap]] or [[Tactics && Techniques && Procedures (TTPs) Phases/Information Gathering/02 - Active Fingerprinting/Port Scanners/Naabu|Naabu]] to discover DBMS open ports.

### Initial Foothold

#### Webshell

TODO: Write more about this

##### Enumerate Web Root Directory

- Discover and enumerate the web root directory.
	- [ ] Find syntax errors or warning messages to find a web root directory.
	- [ ] A directory path where to upload files.

##### Upload File

- [ ] After successfully exploiting SQL injection. Upload a webshell if possible.

### Exploit Chaining

- [ ] If **SQLMap** detects the SQL injection parameters with positive heuristic checks of XSS. Craft an [[Tactics && Techniques && Procedures (TTPs) Phases/WebApp Hacking/Injection/Cross Site Scripting (XSS)/Lab Simulation/01 - DVWA/Stored XSS/Difficulty/Low/Manual#^a7f343|XSS payload]] then insert it in the database to steal cookie sessions.