# Metasploit

```
msf > use auxiliary/crawler/msfcrawler

msf auxiliary(crawler/msfcrawler) > options 

Module options (auxiliary/crawler/msfcrawler):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   PATH     /                yes       Starting crawling path
   RHOSTS                    yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT    80               yes       Remote port
   THREADS  1                yes       The number of concurrent threads (max one per host)


View the full module info with the info, or info -d command.

msf auxiliary(crawler/msfcrawler) > advanced 

Module advanced options (auxiliary/crawler/msfcrawler):

   Name                 Current Setting                                  Required  Description
   ----                 ---------------                                  --------  -----------
   CrawlerModulesDir    /usr/share/metasploit-framework/data/msfcrawler  yes       The base directory containing the crawler modules
   DontCrawl            .exe,.zip,.tar,.bz2,.run,.asc,.gz                yes       Filestypes not to crawl
   EnableUl             true                                             no        Enable maximum number of request per URI
   MaxUriLimit          10                                               yes       Number max. request per URI
   ReadTimeout          3                                                yes       Read timeout (-1 forever)
   ShowProgress         true                                             yes       Display progress messages during a scan
   ShowProgressPercent  10                                               yes       The interval in percent that progress should be shown
   SleepTime            0                                                yes       Sleep time (secs) between requests
   StoreDB              false                                            no        Store requests in database
   TakeTimeout          15                                               yes       Timeout for loop ending
   ThreadNum            20                                               yes       Threads number
   VERBOSE              false                                            no        Enable detailed status messages
   WORKSPACE                                                             no        Specify the workspace for this module


View the full module info with the info, or info -d command.

msf auxiliary(crawler/msfcrawler) > set dontcrawl .exe,.run,.asc,.jpg,.jpeg,.png,.tiff,.woff,.woff2,.webp
```

---
## References

- [Hacking Articles: 5 Ways to Crawl a Website](https://www.hackingarticles.in/5-ways-crawl-website/)