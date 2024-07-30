Установить клиента на Windows 
Проверить

```
# zabbix_agentd -p

# zabbix_agentd -p | grep agent.version
```

Настройка авторегистрации систем с агентами, работающими в активном режиме
```
Alert - Actions - Auto registration 
  Name: Add Windows clients                                         
  Conditions: Host name contains CLIENT                             
  Action operations: 
    Add to host groups: Windows clients                              
    Link to templates: Windows by Zabbix agent active                
                     
  Set host inventory mode: Automatic
```

  Настройка агента на активный режим
  ```
LogFile=C:\Program Files\Zabbix Agent\zabbix_agentd.log
ListenIP=0.0.0.0
StartAgents=0
ServerActive=<ip server>
Hostname=CLIENTN
```
