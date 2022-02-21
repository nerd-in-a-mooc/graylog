# 🐃 Winlogbeat

Winlogbeat is one of the beat agents from Elasticsearch made to send logs over the network. 

## 🍅 Installation

Download the x64 agent.ZIP from [here](https://www.elastic.co/fr/downloads/beats/winlogbeat) !
Unzip the archive under `C:\Program Files (x86)\Winlogbeat`

## 🥔 Configuration

Save the original configuration file - in case you want to roll back, or just check it.

And try out [that configuration file](winlogbeat.yml).

**Then open a PowerShell in that folder**
```PowerShell
.\install-service-winlogbeat.ps1
```

**In case the script doesn't want to properly work**
```PowerShell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```
## 🌽 Start the service

```cmd
net start winlogbeat
```

## 🧄 Check the service

You should see log coming to your Graylog server.

If not, please check our FAQ.