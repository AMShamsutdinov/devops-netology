1) Работа c HTTP через телнет.
Подключитесь утилитой телнет к сайту stackoverflow.com telnet stackoverflow.com 80
отправьте HTTP запрос
```commandline
  GET /questions HTTP/1.0
  HOST: stackoverflow.com
  [press enter]
  [press enter]
```
В ответе укажите полученный HTTP код, что он означает?

Ответ:
```commandline
  vagrant@vagrant:~$ telnet stackoverflow.com 80
  Trying 151.101.129.69...
  Connected to stackoverflow.com.
  Escape character is '^]'.
  GET /questions HTTP/1.0
  HOST: stackoverflow.com
  HTTP/1.1 301 Moved Permanently
  cache-control: no-cache, no-store, must-revalidate
  location: https://stackoverflow.com/questions
  x-request-guid: d637034c-1e3d-4b40-b8d8-f189e4d41eb2
  feature-policy: microphone 'none'; speaker 'none'
  content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
  Accept-Ranges: bytes
  Date: Wed, 16 Feb 2022 20:13:46 GMT
  Via: 1.1 varnish
  Connection: close
  X-Served-By: cache-hel1410031-HEL
  X-Cache: MISS
  X-Cache-Hits: 0
  X-Timer: S1645042426.202281,VS0,VE110
  Vary: Fastly-SSL
  X-DNS-Prefetch-Control: off
  Set-Cookie: prov=ae469b49-0da5-76e5-bec4-cc03dcb5667b; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly

  Connection closed by foreign host.
```

Получен код 301 Moved Permanently. Данный ответ означает что страница http://stackoverflow.com/questions была перемещена на https://stackoverflow.com/questions.

2) Повторите задание 1 в браузере, используя консоль разработчика F12

Ошибка 301 и перенаправление:
![image](https://user-images.githubusercontent.com/95446190/155805684-11c40159-ee8b-4a77-b92f-14fb2936a6ee.png)

Самый долгий запрос 383ms
![image](https://user-images.githubusercontent.com/95446190/155806043-c9168793-2899-412e-83b0-09f4d92cfbed.png)

Описание запроса:
![image](https://user-images.githubusercontent.com/95446190/155806119-f5cb008b-6eff-4ffc-bfe8-23bbe05cb412.png)

3) Какой IP адрес у вас в интернете?

Мой ip:46.191.232.196
![image](https://user-images.githubusercontent.com/95446190/155806713-d16e5c5e-2162-42eb-822c-5917a7de6b98.png)

4) Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой `whois`

  Имя провайдера: JSC "Ufanet"
  Автономная система: AS24955
  
```
    root@vagrant:/home/vagrant# whois 46.191.232.196
    % This is the RIPE Database query service.
    % The objects are in RPSL format.
    %
    % The RIPE Database is subject to Terms and Conditions.
    % See http://www.ripe.net/db/support/db-terms-conditions.pdf
    % Note: this output has been filtered.
    %       To receive output for a database update, use the "-B" flag.
    % Information related to '46.191.232.0 - 46.191.239.255'
    % Abuse contact for '46.191.232.0 - 46.191.239.255' is 'abuse@ufanet.ru'
    inetnum:        46.191.232.0 - 46.191.239.255
    netname:        UBN
    descr:          JSC "Ufanet"
    descr:          Ufa, Russia
    country:        RU
    admin-c:        UN1646-RIPE
    tech-c:         UN1646-RIPE
    status:         ASSIGNED PA
    mnt-by:         UBN-MNT
    created:        2011-11-18T13:00:19Z
    last-modified:  2018-08-02T03:10:23Z
    source:         RIPE

    role:           Ufanet NOC
    address:        pr. Oktyabrya, 4/3
    address:        Ufa, Russia
    org:            ORG-Zs2-RIPE
    admin-c:        AS39184-RIPE
    tech-c:         VO1179-RIPE
    tech-c:         RK10446-RIPE
    abuse-mailbox:  abuse@ufanet.ru
    nic-hdl:        UN1646-RIPE
    mnt-by:         UBN-MNT
    created:        2018-06-06T11:54:33Z
    last-modified:  2019-12-18T11:54:01Z
    source:         RIPE # Filtered

    % Information related to '46.191.232.0/24AS24955'

    route:          46.191.232.0/24
    descr:          JSC "Ufanet", Ufa, Russia
    origin:         AS24955
    mnt-by:         UBN-MNT
    created:        2018-08-15T03:36:56Z
    last-modified:  2018-08-15T03:36:56Z
    source:         RIPE

    % This query was served by the RIPE Database Query Service version 1.102.2 (HEREFORD)
```
