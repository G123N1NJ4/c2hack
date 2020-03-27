# IPV6

## Local IPV6 Enumeration
```
ping6 -c 5 ff02::1%eth0 >/dev/null
ip -6 neigh

detect-new-ip6 eth0

nmap -6 -sS -sC fc00:660:0:1::23
```

## Remote IPv6
miredo: launch the daemon in order to use ipv6 tunneling

socat proxy with IPv6 target:

```
socat TCP-LISTEN:PORT,reuseaddr,fork TCP6:[fe80::6aa8:6dff:fe40:9864]:80
```

Then command on localhost:8080

## Example ipv6 discovery and scan on local network
```
ifconfig eth0 | grep inet6
	inet6 fe80::c8d5:c5ff:fe66:8013  prefixlen 64  scopeid 0x20<link>
    inet6 fc00:660:0:1:c8d5:c5ff:fe66:8013  prefixlen 64  scopeid 0x0<global>
```

```
ip -6 neigh
```

```
ping6 -I fcc00:660:0:1:20c:29ff:fecb:5e:06 -c 3 ff02::1%eth0 >/dev/null
```

```
ip -6 neigh 
fe80::250:56ff:fead:720b dev tap0 lladdr 00:50:56:ad:72:0b REACHABLE
fe80::250:56ff:fead:49b8 dev tap0 lladdr 00:50:56:ad:49:b8 REACHABLE
fc00:660:0:1:413a:8f1:1e2e:b166 dev tap0 lladdr 00:50:56:ad:72:0b REACHABLE
fe80::250:56ff:fead:6c52 dev tap0 lladdr 00:50:56:ad:6c:52 REACHABLE
fc00:660:0:1::46 dev tap0 lladdr 00:50:56:ad:75:d6 router REACHABLE
fc00:660:0:1::44 dev tap0 lladdr 00:50:56:ad:49:b8 REACHABLE
fc00:660:0:1:250:56ff:fead:75ce dev tap0 lladdr 00:50:56:ad:75:ce REACHABLE
fc00:660:0:1:5156:270d:f135:8d50 dev tap0 lladdr 00:50:56:ad:6c:52 REACHABLE
fe80::250:56ff:fead:75d6 dev tap0 lladdr 00:50:56:ad:75:d6 router REACHABLE
fe80::250:56ff:fead:75ce dev tap0 lladdr 00:50:56:ad:75:ce REACHABLE
```

```
nmap -sS 10.10.10.70

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2019-08-21 06:26 PDT
Nmap scan report for kittenwar.sec660.org (10.10.10.70)
Host is up (0.13s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
443/tcp open  https
MAC Address: 00:50:56:AD:75:D6 (VMware)


```

```
nmap -6 -sS fc00:660:0:1::46

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2019-08-21 06:29 PDT
Nmap scan report for fc00:660:0:1::46
Host is up (0.20s latency).
Not shown: 998 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
110/tcp open  pop3
MAC Address: 00:50:56:AD:75:D6 (VMware)


```

```
socat TCP-LISTEN:110,reuseaddr,fork TCP6:[fc00:660:0:1::46]:110
```

