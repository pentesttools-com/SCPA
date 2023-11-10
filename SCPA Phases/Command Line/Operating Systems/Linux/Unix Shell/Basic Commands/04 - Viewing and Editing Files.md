# 04 - Viewing and Editing Files

## 4.1 - Less

- **Read file with ANSI characters**

`$ less -r file.txt`

## 4.2 - Sed

### 4.2.1 - Parse Files

- Remove ANSI characters

```
$ sed -r "s/\x1B\[(([0-9]+)(;[0-9]+)*)?[m,K,H,f,J]//g" file.txt

$ sed 's/\x1b\[[0-9;]*m//g' file.txt
```

- **Remove empty lines**

`$ sed -i "/^$/d" file.txt`

---
## References

- [Grep Multiple Strings](https://phoenixnap.com/kb/grep-multiple-strings)