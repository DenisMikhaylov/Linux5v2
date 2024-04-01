

Установить агента на gate

Процедура установки последней версии агента на сайтек Zabbix
```
https://www.zabbix.com/download?zabbix=6.4&os_distribution=debian&os_version=11&components=agent&db=&ws=
```


Упращенная инсталяция агента на Debian


Установка агента Zabbix Обновление базы данных пакетов и установка агента Zabbix:
```
sudo apt update
sudo apt install zabbix-agent
```
Шаг 1. Настройка агента Откройте файл конфигурации агента Zabbix:
```
sudo nano /etc/zabbix/zabbix_agentd.conf
```
Обновите параметры Server , чтобы они указывали на IP-адрес вашего сервера Zabbix:
```
Server=<Zabbix_Server_IP>
```
Шаг 2: Запуск агента Включите и запустите службу агента Zabbix:
```
sudo systemctl enable zabbix-agent
sudo systemctl start zabbix-agent
```

Установка агента в Windows

Установка агента Zabbix Обновление базы данных пакетов и установка агента Zabbix:
```
https://www.zabbix.com/download_agents
```
Шаг 1. Загрузка программы установки агента Zabbix Начните с загрузки программы установки агента Zabbix для Windows с официального сайта Zabbix.


Шаг 2. Установка агента Найдите и запустите загруженную программу установки от имени администратора, следуя инструкциям на экране. Во время установки укажите IP-адрес сервера Zabbix.


Шаг 3. Настройка агента Перейдите к файлу конфигурации агента Zabbix, который обычно находится по адресу C:\Program Files\Zabbix Agent\zabbix_agentd.conf. Откройте файл конфигурации с помощью текстового редактора и измените параметры Server и ServerActive так, чтобы они указывали на IP-адрес вашего сервера Zabbix.


Шаг 4: Запуск агента как службы Windows Зайдите в приложение "Службы", нажав Win + R, набрав services.msc и нажав Enter. Найдите службу Zabbix Agent и запустите ее.
Выполнив эти шаги, вы успешно установили и настроили агент Zabbix на системе Windows, что позволило ему передавать важные метрики на сервер Zabbix.


Подключитсья к Debian :

Получить список элементов агента
```
# zabbix_agentd -p
```
```
# zabbix_agentd -p | grep agent.version
```
```
# zabbix_agentd -p | grep vm.memory.size
```
```
# zabbix_agentd -t vm.memory.size[available]
```
```
# cat /proc/meminfo | grep MemAvailable
```
```
# zabbix_agentd -t system.sw.packages
```

Проверка получения данных zabbix сервера с агентов

Устанавливаем утилиту на zabbix сервер

```
apt-get install zabbix-get
```
Проверка связи с агентом

```
zabbix_get -s IP/DNSNAME -p 10050 -k agent.version

```


