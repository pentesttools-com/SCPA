# ASNmap

## Setup

```
$ go install github.com/projectdiscovery/asnmap/cmd/asnmap@latest && \
sudo cp ~/go/bin/asnmap /usr/local/bin/
```

## Help Menu

```
$ asnmap -h
Usage:
  ./asnmap [flags]

Flags:
INPUT:
   -a, -asn string[]     target asn to lookup, example: -a AS5650
   -i, -ip string[]      target ip to lookup, example: -i 100.19.12.21, -i 2a10:ad40::
   -d, -domain string[]  target domain to lookup, example: -d google.com, -d facebook.com
   -org string[]         target organization to lookup, example: -org GOOGLE
   -f, -file string[]    targets to lookup from file

CONFIGURATIONS:
   -config string           path to the asnmap configuration file
   -r, -resolvers string[]  list of resolvers to use

OUTPUT:
   -o, -output string  file to write output to
   -j, -json           display json format output
   -c, -csv            display csv format output
   -v6                 display ipv6 cidr ranges in cli output
   -v, -verbose        display verbose output
   -silent             display silent output
   -version            show version of the project
```

## Usage

- Lookup target via ASN

`$ asnmap -a <asn_ID_1>,<asn_ID_2>,<asn_ID_n> -o output.txt`

- Lookup target via IP

`$ asnmap -i <IP_1>,<IP_2>,<IP_n> -o output.txt`

- Lookup target via domain

`$ asnmap -d <domain_1>,<domain_2>,<domain_n> -o output.txt`

- Lookup target via organization

`$ asnmap -org <organization_1>,<organization_2>,<organization_n> -o output.txt`

- Lookup targets list

`$ asnmap -f targets.txt -o output.txt`