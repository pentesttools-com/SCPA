# 02 - File and Directory Manipulation

Search Tag(s): #command-line #windows

## 2.1 - Create Files and Directories

### 2.1.1 - Create an empty file

`C:\> con file.txt`

### 2.1.2 - Create a directory

`C:\> mkdir new_directory`

## 2.2 - Text Editor

Note: Press **CTRL + Z** to end the file

```
C:\> copy con file.txt
This is a text file
Another line.^Z
```

## 2.3 - File and Directory Attribution

TODO: Fill this info

- Set the attributes by flagging it as hidden (`+h`), read only (`+r`), and important system file (`+s`)

`C:\> attrib +h +r +s <file | directory>`

- Clear the attributes the by flagging them hidden (`-h`), read only (`-r`), and important system file (`-s`)

`C:\> attrib -h -r -s <file | directory>`

---
## References

- [[Windows Command Prompt References]]