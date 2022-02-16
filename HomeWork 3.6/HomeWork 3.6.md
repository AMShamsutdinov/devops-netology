1) Работа c HTTP через телнет.
Подключитесь утилитой телнет к сайту stackoverflow.com telnet stackoverflow.com 80
отправьте HTTP запрос

  GET /questions HTTP/1.0
  HOST: stackoverflow.com
  [press enter]
  [press enter]
  
В ответе укажите полученный HTTP код, что он означает?

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
