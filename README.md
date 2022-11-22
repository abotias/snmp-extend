# SNMP EXTEND
How to monitor the system with a script via SNMP.
This example monitors memory but can monitor any parameter.

This script returns memory usage in percentage:

```sh
#!/bin/bash
if [[ "$1" == "mem" ]];then free |grep Mem|awk '{print $3/$2 * 100}'|awk -F"." '{printf $1}';fi
```
Add to file /etc/snmp/snmpd.conf:
```sh
extend checkmem /var/prtg/snmp/checksystem.sh mem
```
Restart service snmpd.

Configure PRTG:

Configure SNMP on the device or group where the devices to be monitored are located:
![Screenshot](assets/image-1.png)

Add sensor "SNMP Custom":
![Screenshot](assets/image-2.png)

Configure sensor:
To know the OID number we can use the snmptranslate tool:
```sh
snmptranslate -On NET-SNMP-EXTEND-MIB::nsExtendOutput1Line .1.3.6.1.4.1.8072.1.3.2.3.1.
```
![Screenshot](assets/image-3.png)

Edit channel to define alert thresholds:\
![Screenshot](assets/image-4.png)

Result:\
![Screenshot](assets/image-5.png)
