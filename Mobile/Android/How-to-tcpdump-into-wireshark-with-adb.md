# How to tcpdump into wireshark with adb

```
adb shell "tcpdump -s 0 w | nc -l -p 4444"
adb forward tcp:4444 tcp:4444
nc localhost 4444 | sudo wireshark -k -S -i -
```
