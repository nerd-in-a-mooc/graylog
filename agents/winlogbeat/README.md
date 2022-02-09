Download the x64 agent.ZIP from [here](https://www.elastic.co/fr/downloads/beats/winlogbeat) !
Unzip the archive under `C:\Program Files (x86)\Winlogbeat`

Save the original configuration file - in case you want to roll back, or just check it.

**Then open a PowerShell in that folder**
```PowerShell
.\install-service-winlogbeat.ps1
```

**In case the script doesn't want to properly work**
```PowerShell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```
