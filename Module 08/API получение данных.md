Настройка подключение к API 

Создание ключа для API

```
Users
  Api tokens
    Create API token
      Name: api test
      User: select Admin
      Set expiration date and time: disable
    
```
```
copy auth token: example(bfa05ad936344b1eeddc053b50a8927210e0a23f6ca03c7c1a8fbe9541fe6523)
```

Установка утилити для обращения

```
# apt install curl
```


Получение узлов из zabbix
```
# export AUTH=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
```
nano /root/zab_get_hosts.sh
```
```
#!/bin/sh

curl -s -k -X POST -H 'Content-Type: application/json-rpc' -d "
{
    \"jsonrpc\": \"2.0\",
    \"method\": \"host.get\",
    \"params\": {},
    \"auth\": \"${AUTH}\",
    \"id\": 2
} " http://127.0.0.1/zabbix/api_jsonrpc.php
```
```
# /root/zab_get_hosts.sh | jq
```
Список имен узлов

```
# /root/zab_get_hosts.sh | jq '.result | .[] | .name'
```
Получение списка карт и их элементов из Zabbix

```
# nano /root/zab_get_maps.sh
```
```
#!/bin/sh

curl -s -k -X POST -H 'Content-Type: application/json-rpc' -d "
{
    \"jsonrpc\": \"2.0\",
    \"method\": \"map.get\",
    \"params\": {
        \"selectLinks\": \"extend\",
        \"selectSelements\": \"extend\"
    },
    \"auth\": \"${AUTH}\",
    \"id\": 2
} " http://127.0.0.1/zabbix/api_jsonrpc.php

```
```
# /root/zab_get_maps.sh | jq -c '.result | .[] | {name: .name, id: .sysmapid}'
```

Пример изменения конфигурации через Zabbix API

```
# nano /root/zab_set_map_name.sh
```

```
#!/bin/sh

MAPID=$1
MAPNAME=$2

curl -s -k -X POST -H 'Content-Type: application/json-rpc' -d "
{
    \"jsonrpc\": \"2.0\",
    \"method\": \"map.update\",
    \"params\": {
        \"sysmapid\": \"${MAPID}\",
        \"name\": \"${MAPNAME}\"
    },
    \"auth\": \"${AUTH}\",
    \"id\": 2
} " http://127.0.0.1/zabbix/api_jsonrpc.php
```
```
# /root/zab_set_map_name.sh <id> <Name map>

