# Checklist

Search Tag(s): #checklist #file-upload

TODO: Provide a good checklist for file upload vulnerability

## Information Gathering

### Passive Reconnaissance

#### Discover Hidden Parameters

- [ ] Find common file upload forms via [[Pentesting Phases/Information Gathering/Passive Fingerprinting/OSINT/Search Engines Dorking/Google Dorking/Dorks/Web Enumeration/|google dorks]].

### Active Reconnaissance

#### Active Enumeration

##### Discover Ports

- [ ] Probing ports to check what service version of the webservers using [[06 - Scan Techniques|nmap]].

##### Fuzz Hidden Parameters

TODO: I got this covered - Userware

- [ ] To bruteforce hidden parameters use [[Tactics && Techniques && Procedures (TTPs) Phases/Information Gathering/02 - Active Fingerprinting/Enumeration/Fuzzers/Bruteforce Parameters/FFuF|FFuF]] or [[Tactics && Techniques && Procedures (TTPs) Phases/Information Gathering/02 - Active Fingerprinting/Enumeration/Fuzzers/Bruteforce Parameters/Wfuzz|Wfuzz]].
- [ ] [[Arjun]] and [[x8]] and [[Tactics && Techniques && Procedures (TTPs) Phases/Information Gathering/01 - Passive Fingerprinting/Enumeration/HTTP/Wayback Machine/Metasploit|Metasploit]].

## Vulnerability Assessment

### Scanners

#### General

- [ ] Run an [[Tactics && Techniques && Procedures (TTPs) Phases/WebApp Hacking/File Upload/Scanners/Nmap|NSE script]] to probe for file upload vulnerabilities on the webservers with `-iL ips.txt` flag.
- [ ] Run [[Tactics && Techniques && Procedures (TTPs) Phases/Vulnerability Assessment/Web Vulnerability Scanner/Nikto|Nikto]] scanner to discover for existing file upload exploits.

#### Content Management System

- [ ] Run [[WPScan]] to check for vulnerable wordpress plugins or themes related to file upload.
- [ ] Run [[Joomscan]] to check for vulnerable joomla plugins or themes related to file upload.

## Exploit

### Manual Injection on Parameters

#### GET Request

- [ ] Check the URL parameters

#### POST Request

- [ ] Perform username enumeration to craft the SQL injection
- [ ] Check POST JSON parameters
- [ ] POST parameters may contain in SOAP/XML

### Manual Injection on Headers

- [ ] Inject X-Forwarded-For
- [ ] Inject X-Forwarded-Host
- [ ] Inject User-Agent header

### Initial Foothold

#### Evasion

- Change the `Content-Type` header to a normal file. For example:
	- Images
		- [ ] `image/png`
- [ ] Use PHP filewrapper to bypass filter
- Include metadata with `exiftool` with any of these tags:
	- [ ] Comment
	- [ ] Copyright
	- [ ] DocumentName
- [ ] Include PHP payload in the basename of the file. For example: `<?php system($_GET['cmd']); ?>.png`

#### Remote File Inclusion (RFI)

- [ ] Find a file upload form to navigate and execute it with directory traversal