Создание проверки на основе zabbix trapper

создаем item на zabbix 
```
Hosts->zabbix->Items
  Name: my item
    Type: Zabbix trapper
    Key:  my.item
    Allowed hosts: 127.0.0.1,192.168.10.0/24
```
Исталируем приложение 

```
# apt install zabbix-sender
```

Тестируем работу приложения

```
zabbix_sender -z 192.168.10.103 -p 10051 -s <Name host> -k my.item -o 1
```

Переделываем работы получение данных из speedtest

Создаем новый скрипт
```
# nano /root/speedtest.sh
```
```
#!/bin/sh

### speedtest-cli ### result bits/s
MY_RES=`speedtest-cli --csv`
MY_DOWNLOAD=`echo $MY_RES | cut -d',' -f7`
MY_UPLOAD=`echo $MY_RES | cut -d',' -f8`

### speedtest ### result Bytes/s (use preprocess Custom multiplier)
#MY_RES=`speedtest -f csv`
#MY_DOWNLOAD=`echo $MY_RES | cut -d',' -f6`
#Y_UPLOAD=`echo $MY_RES | cut -d',' -f7`

zabbix_sender -z 127.0.0.1 -p 10051 -s <Name host> -k speedtest.download -o $MY_DOWNLOAD
zabbix_sender -z 127.0.0.1 -p 10051 -s <Name host> -k speedtest.upload -o $MY_UPLOAD
```

Создаем новые Item

```
zabbix->Items
  Name: speedtest download trap
    Type: Zabbix trapper
    Key:  speedtest.download
    Type of information: Numeric (float) или Numeric (unsigned)
    Units: bit/s
    Allowed hosts: 127.0.0.1
...
  Name: speedtest upload trap
    Type: Zabbix trapper
    Key:  speedtest.upload
    Type of information: Numeric (float) или Numeric (unsigned)
    Units: bit/s
    Allowed hosts: 127.0.0.1


```

Запускаем скрипт

```
/root/speedtest.sh
```
