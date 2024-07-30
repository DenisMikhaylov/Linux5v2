
Подключаемся к серверу zabbix

Добавить репозитарий apt

```
nano /etc/apt/sources.list
```
```
deb http://deb.debian.org/debian/ bullseye main contrib non-free
deb-src http://deb.debian.org/debian/ bullseye main contrib non-free

deb http://security.debian.org/debian-security bullseye-security main contrib non-free
deb-src http://security.debian.org/debian-security bullseye-security main contrib non-free

deb http://deb.debian.org/debian/ bullseye-updates main contrib non-free
deb-src http://deb.debian.org/debian/ bullseye-updates main contrib non-free

```


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
snmpwalk -On -c public -v2c <ip windows> sysName

```

В место route напишите ip windows
Перебор всех значений OID устройства
```
server# snmpwalk -On -c public -v2c <ip windows> 1
```
Определение имени устройства
```
server# snmpget -c public -v2c <ip windows> .1.3.6.1.2.1.1.5.0

server# snmpget -c public -v2c <ip windows> SNMPv2-SMI::mib-2.1.5.0

server# snmpget -c public -v2c <ip windows> SNMPv2-MIB::sysName.0

server# snmpget -c public -v2c <ip windows> sysName.0

server# snmpwalk -c public -v2c <ip windows> sysName
```
Определение uptime устройства
```
server# snmpget -c public -v2c <ip windows> sysUpTime.0

```
Получение mac address table
```
radio$ snmpwalk -On -v2c -c public@101 <ip windows> 1.3.6.1.2.1.17.4.3.1.2
```
Определение загрузки CPU устройства
```
server# snmpget -c public -v2c <ip windows> .1.3.6.1.4.1.9.2.1.56.0
```
Вывод списка интерфейсов устройства
```
server# snmpwalk -c public -v2c -On <ip windows> ifDescr
```

Вывод количества байт, прошедших через порт устройства с момента его включения
```
FastEthernet0/0

server# snmpget -c public -v2c -On <ip windows> ifInOctets.1
server# snmpget -c public -v2c -On <ip windows> ifHCInOctets.1

Port-channel1

server# snmpget -c public -v2c -On <ip windows> ifOutOctets.5
server# snmpget -c public -v2c -On <ip windows> ifHCOutOctets.5
```
Информация по протоколу CDP
```
server# snmpwalk -c public -v2c <ip windows> .1.3.6.1.4.1.9.9.23.1.2.1.1
```


Настроить автоматичекое добавление
```
Discovery
New Discovery rules
  Name: Local network new
  IP range: 192.168.123.1-254
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
