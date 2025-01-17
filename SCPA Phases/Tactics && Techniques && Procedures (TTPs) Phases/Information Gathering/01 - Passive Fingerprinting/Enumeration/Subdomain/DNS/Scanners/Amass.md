# Amass

`$ amass intel -d <domain.com>`

`$ amass intel -d <domain.com> -whois`

`$ amass intel -org '<orgination_name>'`

`$ amass enum -passive -d <domain.com> -o subdomains.txt`

`$ amass enum -passive -df domains.txt -o subdomains.txt`

`$ amass enum -passive -df domains.txt -o subdomains.txt && awk < subdomains.txt '{ system("resolveip -s "$1)}' > ips.txt`

`$ amass db -df domains.txt -names -o subdomains.txt`

`$ sudo amass track -dir domains-dir-output -d <domaim.com -last 2`

---
## References

- [Amass Tutorial](https://github.com/OWASP/Amass/wiki/Tutorial)

- [Amass User Guide](https://github.com/OWASP/Amass/wiki/User-Guide)

- [Amass Quick Tutorial Example Usage](https://allabouttesting.org/owasp-amass-quick-tutorial-example-usage/)