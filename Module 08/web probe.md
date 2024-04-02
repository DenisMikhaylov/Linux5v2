[Пример разработки](https://www.zabbix.com/documentation/4.0/ru/manual/web_monitoring/example)

Выбери сайт на который вы хотите подключить проверку


Проведите исследование сайт 
```
Шаг 1.

Браузер: <ваш сайт>

view-source
...
...Roundcube Webmail...
...
<input type="hidden" name="_token" value="29JVrZhgW97xID7K2pkSRRHsngGDRGCY">
...

Шаг 2.
Браузер: вводим логин/пароль и нажимаем "Войти"

...
POST /mail/?_task=login HTTP/1.1
...
_token=29JVrZhgW97xID7K2pkSRRHsngGDRGCY&_task=login&_action=login&_timezone=Europe%2FMoscow&_url=&_user=student&_pass=password
...
HTTP/1.1 302 Found
...
Location: ./?_task=mail&_token=pWUje42O61E2Rm0r8zgKzOPXWGby8ugP
...

view-source
...
...button-logout...
...
<input type="hidden" name="_token" value="pWUje42O61E2Rm0r8zgKzOPXWGby8ugP">
...

3.
Браузер: нажимаем "Выход"


...
GET /mail/?_task=logout&_token=pWUje42O61E2Rm0r8zgKzOPXWGby8ugP HTTP/1.1
```


```
Name: Web test
Variables
  {login} <свой логин>
  {password} <свой пароль>

Steps

Step 1
  Name: First page
  URL: <Ваш сайт>

  Variables
    {token1} regex:name="_token" value="([0-9A-Za-z]{32})"
Можно проще:
    {token1} regex:name="_token" value="(.{32})"

  Required string: rcmloginsubmit
  Required status codes: 200
  
Step 2
  Name: Log in
  URL: <Ваш сайт>
  
  Post fields
    _token: {token1}
    _task: login
    _action: login
    _user: {login}
    _pass: {password}

  Variables
    {token2}: regex:name="_token" value="(.{32})"
    
  Follow redirects: YES
  
  Required string: button-logout
  Required status codes: 200
  
Step 3
  Name: Log out
  URL: <Ваш сайт>
  
  Query fields
    _task: logout
    _token: {token2}
    
  Required string: rcmloginsubmit
  Required status codes: 200

```
