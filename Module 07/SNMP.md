
Подключаемся к серверу zabbix

Установка snmp консоли
```
# apt install snmp snmp-mibs-downloader
# :> /etc/snmp/snmp.conf
```


Включение SNMP на Windows 

Добавить компонент Service SNMP и WMI for SNMP

Настроить службу SNMP

На вкладке безопасность добавить имя public

И выставить признак принимать запросы от люого узла

На вкладке Агент SNMP выставить чекбокс для всех служб


Проверям на zabbix server

```
snmpwalk -On -c public -v2c 192.168.10.104 sysName

```

В место route напишите ip windows
Перебор всех значений OID устройства
```
server# snmpwalk -On -c public -v2c router 1
```
Определение имени устройства
```
server# snmpget -c public -v2c router .1.3.6.1.2.1.1.5.0

server# snmpget -c public -v2c router SNMPv2-SMI::mib-2.1.5.0

server# snmpget -c public -v2c router SNMPv2-MIB::sysName.0

server# snmpget -c public -v2c router sysName.0

server# snmpwalk -c public -v2c router sysName
```
Определение uptime устройства
```
server# snmpget -c public -v2c router sysUpTime.0

```
Получение mac address table
```
radio$ snmpwalk -On -v2c -c public@101 switch 1.3.6.1.2.1.17.4.3.1.2
```
Определение загрузки CPU устройства
```
server# snmpget -c public -v2c router .1.3.6.1.4.1.9.2.1.56.0
```
Вывод списка интерфейсов устройства
```
server# snmpwalk -c public -v2c -On router ifDescr
```

Вывод количества байт, прошедших через порт устройства с момента его включения
```
FastEthernet0/0

server# snmpget -c public -v2c -On router ifInOctets.1
server# snmpget -c public -v2c -On router ifHCInOctets.1

Port-channel1

server# snmpget -c public -v2c -On router ifOutOctets.5
server# snmpget -c public -v2c -On router ifHCOutOctets.5
```
Информация по протоколу CDP
```
server# snmpwalk -c public -v2c router .1.3.6.1.4.1.9.9.23.1.2.1.1
```


Настроить автоматичекое добавление
```
Discovery
New Discovery rules
  Name: Local network new
  IP range: 192.168.10.1-254
  Checks: 
    Check type: SNMPv2 agent 
    SNMP community: public
    SNMP OID .1.3.6.1.2.1.1.5.0
    Add
  Update interval: 3m



Alert->Actions-> Discovery action
  Create action
    Name: Action add snmp device to zabbix
    Conditions: 
      Discovery status: equals Up                 
      Add
    Operations:
      Add host
      Add to host groups: Discovered hosts            
      Link to templates:
              1. Windows by SNMP
      Set host inventory mode: Automatic
    Add
```

Дождаться добавление host по SNMP
Проверить в LAtest data собранные данные по SNMP
