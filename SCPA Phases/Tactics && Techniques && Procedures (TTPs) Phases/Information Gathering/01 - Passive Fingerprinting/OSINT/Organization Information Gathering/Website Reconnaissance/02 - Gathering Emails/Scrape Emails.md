# Scrape Emails

## 01 - Manual

**Note:** some websites I've discovered that they couldn't work with `curl` or `wget` in this case go to **View Page Source** on any web browser of your choice and save it as html file then use the `grep` command to parse it.

```
$ wget -qO- http[s]://<IP> | grep -Eo "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,6}\b" | sort -u > emails.txt

$ curl --silent http[s]://<IP> | grep -Eo "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,6}\b" | sort -u > emails.txt

$ grep -Eo "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,6}\b" file.html | sort -u > emails.txt
```

## 02 - CeWL

`$ cewl -u <user_agent> -ne --email_file emails.txt http[s]://<IP>`

## 03 - DataSurgeon

```
$ curl --silent http[s]://<IP> | ds -eC | uniq

$ wget -qO- http[s]://<IP> | ds -eC | uniq
```

## 04 - Recon-ng

- **`whois_pocs` recon-ng module**

```
[recon-ng][default] > marketplace install recon/domains-contacts/whois_pocs

[recon-ng][default] > modules load recon/domains-contacts/whois_pocs

[recon-ng][default][whois_pocs] > options set SOURCE <domain.com>

[recon-ng][default][whois_pocs] > run

[recon-ng][default][whois_pocs] > back
```

- **`pgp_search` recon-ng module**

```
[recon-ng][default] > marketplace install recon/domains-contacts/pgp_search

[recon-ng][default] > modules load recon/domains-contacts/pgp_search

[recon-ng][default][pgp_search] > options set SOURCE <domain.com>

[recon-ng][default][pgp_search] > run

[recon-ng][default][pgp_search] > back
```

## 05 - Metasploit

```
msf > use auxiliary/gather/search_email_collector

msf auxiliary(gather/search_email_collector) > options

Module options (auxiliary/gather/search_email_collector):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   DOMAIN                          yes       The domain name to locate email addresses for
   OUTFILE                         no        A filename to store the generated email list
   SEARCH_BING    true             yes       Enable Bing as a backend search engine
   SEARCH_GOOGLE  true             yes       Enable Google as a backend search engine
   SEARCH_YAHOO   true             yes       Enable Yahoo! as a backend search engine

msf auxiliary(gather/search_email_collector) > set domain <website.com>

msf auxiliary(gather/search_email_collector) > set search_bing <true | false>

msf auxiliary(gather/search_email_collector) > set search_google <true | false>

msf auxiliary(gather/search_email_collector) > set search_yahoo <true | false>

msf auxiliary(gather/search_email_collector) > set outfile /path/to/file.txt

msf auxiliary(gather/search_email_collector) > run
```

---
## References

- [[Tactics && Techniques && Procedures (TTPs) Phases/Initial Access/Password Cracking/Online/Generate Custom Wordlist/CeWL|CeWL]]

- [Hacking Articles: A Detailed Guide on Cewl](https://www.hackingarticles.in/a-detailed-guide-on-cewl/)