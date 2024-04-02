
Мониторинг температуры Windows.
https://habr.com/ru/articles/793296/

Задание 1. Инвентаризация данных
Шаг 1. Настройка сбора инвентарных данных  оборудования

В веб интерфейсе zabbix открывает "Data collection" - 'Host'
```
Host: CLIENTN
  Items
    Name: WMI object disk freespace
    Type: text
    Key: wmi.get[root\cimv2,SELECT name FROM Win32_baseboard]
    Populates host inventory field: model

  Items
    Name: WMI object serial number baseboard
    Type: agent zabbix active
    Key: wmi.get[root\cimv2,SELECT serialnumber FROM Win32_baseboard]
    Populates host inventory field: Chassis
```
Шаг 2. Настройка сбора инвентарных данных  программного обеспечения Windows

Переходим на Windows клиента

открываем на редактирование файл конфигурации агента
``
C:\> C:\Program Files\Zabbix Agent\zabbix_agentd.conf
``
Добавляем строчку
```
UserParameter=listinstalledsoft,C:\bin\listinstalledsoft.bat | findstr /v "^$"
```
Создаем по пути bat файл C:\bin\listinstalledsoft.bat
```
@echo off

powershell -command "Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Get-ItemProperty | Where-Object 'DisplayName' | Sort-Object -Property DisplayName | Select-Object -Property DisplayName | Format-Table -AutoSize -HideTableHeaders"
powershell -command "Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Get-ItemProperty | Where-Object 'DisplayName' | Sort-Object -Property DisplayName | Select-Object -Property DisplayName | Format-Table -AutoSize -HideTableHeaders"

```
В веб интерфейсе zabbix открывает "Data collection" - 'Host'
```
Host: CLIENTN

Items
    Name: Install soft
    Type: text
    Key: listinstalledsoft
    Populates host inventory field: software
```
Шаг 3. Настройка сбора инвентарных данных  программного обеспечения Linux

Подключаемся по ssh на Zabbix servevr

Создаем файл
```
#nano /etc/zabbix/zabbix_agentd.conf.d/listinstalledsoft.conf
```

Добавляем строчку
```
UserParameter=listinstalledsoft,ls /usr/share/applications | awk -F '.desktop' ' { print $1}' –
```
В веб интерфейсе zabbix открывает "Data collection" - 'Host'
```
Host: Zabbix server

Items
    Name: Install soft 
    Type: text
    Key: listinstalledsoft
    Populates host inventory field: software
```
