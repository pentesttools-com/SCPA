# 07 - Help Menu

## 7.1 - Beacon Session

### 7.1.1 - Help Menu

```
sliver > use -h

Command: use [sliver name/session]
About: Switch the active Sliver, a valid name must be provided (see sessions).

Usage:
======
  use [flags] [id]

Args:
=====
  id  string    beacon or session ID

Flags:
======
  -h, --help           display help
  -t, --timeout int    command timeout in seconds (default: 60)

Sub Commands:
=============
  beacons   Switch the active beacon
  sessions  Switch the active session
```

### 7.1.2 - Usage

- Interact a long beacon or session id string.

`sliver > use`