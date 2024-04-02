# Настройка zabbix proxy LTS



Для начала стоить отметить что zabbix-proxy сервер не имеет веб интерфейса, соответственно использует крайне мало системных ресурсов.


```
sudo su 
apt update
apt upgrade
reboot
```
далее нам понадобится текстовый редактор так как в минимальной сборке он не предоставляется ))
```
sudo su
apt install nano
```
Далее идем на сайт заббикса и выбираем нужный дистрибутив
```
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu22.04_all.deb
dpkg -i zabbix-release_6.0-4+ubuntu22.04_all.deb
apt update
apt install zabbix-proxy-sqlite3

```

Прежде чем менять файл с настройками сгенерируем на своем компе psk фразу командой и записываем ее в файл
```
openssl rand -hex 32 > /etc/zabbix/zabbix_proxy.psk
```
Далее выводим этот ключ на экран командой 
```

tail /etc/zabbix/zabbix_proxy.psk
```
Сохраняем где нибудь у себя данный ключ он нам пригодиться при настройки серверной части заббикса
И вот теперь конфигурируем файл с настройками

```
nano /etc/zabbix/zabbix_proxy.conf
```

Меняем следующие значения:\
Server=    ваш ip adresss\
Hostname=  такой же нужно будет прописать на основном сервере\
DBName= /tmp/zabbix_proxy\
TLSAccept=psk
TLSConnect=psk\
TLSPSKFile=/etc/zabbix/zabbix_proxy.psk\
TLSPSKIdentity=test  - тут меняйте значение на свое - такое же должно использоваться на серверной части\

```
systemctl restart zabbix-proxy
systemctl enable zabbix-proxy
```

