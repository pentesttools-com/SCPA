# Manual

```
$ curl --silent http[s]://<IP> | grep -Eo "https?://[a-zA-Z0-9./?=_-]*(:[[:digit:]]+)?" | cut -d "/" -f 3 | sort -u > urls.txt"

$ wget -qO- http[s]://<IP> | grep -Eo "https?://[a-zA-Z0-9./?=_-]*(:[[:digit:]]+)?" | cut -d "/" -f 3 | sort -u > urls.txt"
```

Note: some websites I've discovered that they couldn't work with `curl` or `wget` in this case go to **View Page Source** on any web browser of your choice and save it as html file then use the `grep` command to parse it.

```
$ grep -Eo "https?://[a-zA-Z0-9./?=_-]*(:[[:digit:]]+)?" file.html | awk -F "/" '{print $3}' | sort -u > urls.txt

$ grep -Eo "https?://[a-zA-Z0-9./?=_-]*(:[[:digit:]]+)?" file.html | cut -d "/" -f 3 | sort -u > urls.txt"
```

- Scrape javascript files of URLs.

```
$ curl --silent http[s]://<IP>/file.js | grep -Eo "https?://[a-zA-Z0-9./?=_-]*(:[[:digit:]]+)?" | cut -d "/" -f 3 | sort -u > urls.txt

$ curl --silent http[s]://<IP>/file.js | grep -Eo "https?://[a-zA-Z0-9./?=_-]*(:[[:digit:]]+)?" | awk -F "/" '{print $3}' | sort -u > urls.txt

$ wget -qO- http[s]://<IP>/file.js | grep -Eo "https?://[a-zA-Z0-9./?=_-]*(:[[:digit:]]+)?" | cut -d "/" -f 3 | sort -u > urls.txt"
```