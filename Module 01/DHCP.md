Установка на Server или Gate в зависимости от курса

Установка приложения 
```
# apt install isc-dhcp-server
```

Настройка сетевого интерфейса
```
# nano /etc/default/isc-dhcp-server
```
Указывается сетевой интерфейс для раздачи адресов
```
INTERFACESv4="eth1" 
```

Настройка DHCP 

```
# nano /etc/dhcp/dhcpd.conf
```

Настройка DNS

```
option domain-name "corp.ru";
option domain-name-servers <ip address server dns>;
```
Настройка Scope
```
shared-network LAN1 {
  subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.2 192.168.1.254;
    option routers 192.168.1.1;
  }
}
```

Проверка работы
```
# dhcpd -t

# service isc-dhcp-server restart

# service isc-dhcp-server status
```

Мониторинг работы DHCP

```
# dhcp-lease-list
# less /var/lib/dhcp/dhcpd.leases
# grep dhcp /var/log/syslog
```

Статитска работы 
```
# apt install dhcpd-pools
# dhcpd-pools
# dhcpd-pools -l /var/lib/dhcp/dhcpd.leases -c /etc/dhcp/dhcpd.conf

```
