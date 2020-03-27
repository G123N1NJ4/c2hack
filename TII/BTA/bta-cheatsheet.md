# BTA Cheatsheet

## Install
TODO

## Import the ntds.dit
```
btaimport -C ::{domain} ntds.dit
```

## Command examples
```
btamanage ls
btaminer
btaminer -C ::{domain} -t ReST ListGroup --match "Domain Admin"
btaminer -C ::{domain} -t ReST Audit_Full
btaminer -C ::{domain} -t excel Audit_Full
btaminer -C ::{domain} -t excel -o output.xlsx Audit_Full

AdminSDHolder, Search for suspicious Users and Groups
btaminer -C::{domain} SDProp --list
btaminer -C ::{domain} SDProp --checkACE

User-Force-Change-Password
btaminer -C ::{domain} ListACE --type 00299570-246d-11d0-a768-00aa006e0529
btaminer -C ::{domain} Audit_ExtRights
btaminer -C ::{domain} passwords --never-logged
btaminer -C ::{domain} passwords --last-logon {days}
btaminer -C ::{domain} passwords --bad-password-count
btaminer -C ::{domain} CheckUAC {param} (dontExpirePassword, accountDisable, passwdCantChange, passwordExpired, lockout, passwordNotRequired...)
btaminer -C ::{domain} Audit_UAC
btaminer -C ::{domain} NewAdmin --creation 2018-01-01
btaminer -C ::{domain} Membership --all-groups
```



## Links
https://bitbucket.org/iwseclabs/bta/src/92cf197efc87238fa32c27a9e3addee1da928564/bta/miners/?at=master
