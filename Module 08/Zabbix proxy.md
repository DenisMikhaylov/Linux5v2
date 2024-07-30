# Настройка zabbix proxy

Подключаемся к серверу Debian-2

Установка MySQL для сервера Zabbix
```
apt update

apt install default-mysql-server
```

Далее идем на сайт заббикса и выбираем нужный дистрибутив и производим установку. Выюираем дистрибутив с MySQL.
Пример. Желательно пройти на сайт и там выбираем подходящую последовательность.
```
https://www.zabbix.com/ru/download?zabbix=6.4&os_distribution=debian&os_version=12&components=proxy&db=mysql&ws=
```
Подключение Proxy Можно произвести с шифрованием или без шифрования.

В нашем примере используем с шифрованием.

Шифрованное подключение

Сгенерируем на сервере psk
```
openssl rand -hex 32 > /etc/zabbix/zabbix_proxy.psk
```
Далее выводим этот ключ на экран командой 
```

tail /etc/zabbix/zabbix_proxy.psk
```
Сохраняем где нибудь у себя данный ключ, он нам пригодится при настройки серверной части заббикса.
Настройка конфигурации.

```
nano /etc/zabbix/zabbix_proxy.conf
```

Меняем следующие значения:\
```
Server=    ваш ip adresss\
Hostname=  такой же нужно будет прописать на основном сервере\
ProxyMode=1
DBPassword=password
TLSAccept=psk
TLSConnect=psk\
TLSPSKFile=/etc/zabbix/zabbix_proxy.psk\
TLSPSKIdentity=test  - тут меняйте значение на свое - такое же должно использоваться на серверной части\
```
Перезапускаем прокси
```
systemctl restart zabbix-proxy
systemctl enable zabbix-proxy
```

Подключение прокси к заббикс

Открываем  в браузере заббикс.

```
Administration -> Proxies
Create proxy
Proxy name: Имя сервера на который установили прокси
Proxy mode: Зависит от выбрано режима
Proxy address: ip debian-2
Переключаемся на вкладку Encryption
Connections from proxy: psk
PSK identity: указываем что и в файле настроек
PSK: текст из файла PSK
```
