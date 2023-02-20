## 1. Работа с HTTP через телнет.
- Подключитесь утилитой телнет к сайту stackoverflow.com telnet stackoverflow.com 80
- Отправьте HTTP запрос
```bash
$telnet stackoverflow.com 80
Trying 151.101.1.69...
Connected to stackoverflow.com.
Escape character is '^]'.
GET /questions HTTP/1.0

HOST: stackoverflow.com

[press enter]

[press enter]
HTTP/1.1 500 Domain Not Found
Connection: close
Content-Length: 220
Server: Varnish
Retry-After: 0
content-type: text/html
Cache-Control: private, no-cache
X-Served-By: cache-bma1660-BMA
Accept-Ranges: bytes
Date: Mon, 20 Feb 2023 15:50:53 GMT
Via: 1.1 varnish


<html>
<head>
<title>Fastly error: unknown domain </title>
</head>
<body>
<p>Fastly error: unknown domain: . Please check that this domain has been added to a service.</p>
<p>Details: cache-bma1660-BMA</p></body></html>Connection closed by foreign host.
```
Получил код 500, который указывает, что сервер не может обработать этот запрос. Это любая внутренняя ошибка сервера, которая не входит в рамки остальных ошибок класса. Появился в HTTP/1.0.


## 2. Повторите задание 1 в браузере, используя консоль разработчика F12.

- откройте вкладку Network
- отправьте запрос http://stackoverflow.com
- найдите первый ответ HTTP сервера, откройте вкладку Headers
- укажите в ответе полученный HTTP код 
  - Первый ответ сервера вернул код 301 Moved Permanently.  запрошенный документ был окончательно перенесен на новый URI, указанный в поле Location заголовка.
- проверьте время загрузки страницы, какой запрос обрабатывался дольше всего? 
  - Время загрузки страницы - 7,92 секунд. Запрос, обрабатываемый дольше всех - TLS Setup - 3976 ms.
- приложите скриншот консоли браузера в ответ.

<image src="1.jpg">

## 3. Какой IP адрес у вас в интернете?
С помощью сервиса whoer.net узнал ip-адрес - 85.249.167.236 


## 4. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? 
```
$ whois 85.249.167.236
% This is the RIPE Database query service.
% The objects are in RPSL format.
%
% The RIPE Database is subject to Terms and Conditions.
% See http://www.ripe.net/db/support/db-terms-conditions.pdf

% Note: this output has been filtered.
%       To receive output for a database update, use the "-B" flag.

% Information related to '85.249.0.0 - 85.249.255.255'

% Abuse contact for '85.249.0.0 - 85.249.255.255' is 'abuse-b2b@beeline.ru'

inetnum:        85.249.0.0 - 85.249.255.255
netname:        RU-SOVINTEL-20050124
org:            ORG-ES15-RIPE
country:        RU
admin-c:        SVNT2-RIPE
tech-c:         SVNT1-RIPE
status:         ALLOCATED PA
mnt-by:         RIPE-NCC-HM-MNT
mnt-by:         SOVINTEL-MNT
mnt-lower:      ELTEL-RIPE-MNT
mnt-lower:      SOVINTEL-MNT
mnt-routes:     ELTEL-RIPE-MNT
mnt-routes:     SOVINTEL-MNT
mnt-domains:    SOVINTEL-MNT
created:        2005-01-24T11:44:05Z
last-modified:  2016-10-27T13:06:35Z
source:         RIPE

organisation:   ORG-ES15-RIPE
org-name:       PJSC "Vimpelcom"
country:        RU
org-type:       LIR
remarks:        VEON Group
address:        8 Marta str, 10, bld. 14
address:        127083
address:        Moscow
address:        RUSSIAN FEDERATION
phone:          +74999233775
fax-no:         +74957871990
admin-c:        SVNT1-RIPE
admin-c:        SVNT2-RIPE
admin-c:        IAI1-RIPE
admin-c:        DM3740-RIPE
admin-c:        BEE15-RIPE
mnt-ref:        SOVINTEL-MNT
mnt-ref:        ROSNIIROS-MNT
mnt-ref:        RIPE-NCC-HM-MNT
mnt-by:         RIPE-NCC-HM-MNT
mnt-by:         SOVINTEL-MNT
abuse-c:        SVNT2-RIPE
created:        2004-04-17T11:58:43Z
last-modified:  2022-04-21T15:45:58Z
source:         RIPE # Filtered

role:           Sovintel NOC
remarks:        now PAO Vimpelcom - formely Sovam Teleport/Teleross
remarks:        aka Sovintel - Golden Telecom
address:        111250 Russia Moscow Krasnokazarmennaya, 12
mnt-by:         SOVINTEL-MNT
org:            ORG-ES15-RIPE
phone:          +7 800 7008061
fax-no:         +7 495 7871010
abuse-mailbox:  abuse-b2b@beeline.ru
admin-c:        IAI1-RIPE
admin-c:        DM3740-RIPE
tech-c:         DM3740-RIPE
tech-c:         SVNT2-RIPE
nic-hdl:        SVNT1-RIPE
created:        2004-05-13T11:50:32Z
last-modified:  2022-04-20T08:31:22Z
source:         RIPE # Filtered

role:           Sovintel Abuse Department
remarks:        now Vimpelcom Business Abuse Department
address:        111250 Russia Moscow, Krasnokazarmennaya, 12
org:            ORG-ES15-RIPE
fax-no:         +7 495 7254300
phone:          +7 495 7871000
nic-hdl:        SVNT2-RIPE
admin-c:        SVNT1-RIPE
tech-c:         SVNT1-RIPE
mnt-by:         SOVINTEL-MNT
created:        2004-05-14T10:21:01Z
last-modified:  2018-11-08T08:46:48Z
source:         RIPE # Filtered
abuse-mailbox:  abuse-b2b@beeline.ru

% Information related to '85.249.160.0/20AS16345'

route:          85.249.160.0/20
descr:          PJSC "VimpelCom"
origin:         AS16345
mnt-by:         AS3216-MNT
created:        2019-10-17T14:08:33Z
last-modified:  2019-10-17T14:08:33Z
source:         RIPE

% This query was served by the RIPE Database Query Service version 1.106 (DEXTER)
```
Мой ip-адрес принадлежит провайдеру Совинтел и автономной системе AS16345.

## 5. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS?
```
$ traceroute -An 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  192.168.134.2 [*]  0.213 ms  0.125 ms  0.193 ms
 2  * * *
 3  * * *
 4  * * *
 5  * * *
 6  * * *
 7  * * *
 8  * * *
 9  * * *
10  * * *
11  * * *
12  * * *
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  * * *
23  * * *
24  * * *
25  * * *
26  * * *
27  * * *
28  * * *
29  * * *
30  * * *
```       
К сожалению, ничего узнать не удалось(

## 6. Повторите задание 5 в утилите mtr. На каком участке наибольшая задержка - delay?

<image src="2.jpg">

У меня mtr выводил только это, я не знаю в чем причина.

## 7. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи?
```
$ dig +short NS dns.google
ns4.zdns.google.
ns3.zdns.google.
ns2.zdns.google.
ns1.zdns.google.

$ dig +short A dns.google
8.8.4.4
8.8.8.8
```

## 8. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP?
```
 dig -x 8.8.4.4

; <<>> DiG 9.18.1-1-Debian <<>> -x 8.8.4.4
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 20881
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; MBZ: 0x0005, udp: 1280
;; QUESTION SECTION:
;4.4.8.8.in-addr.arpa.          IN      PTR

;; ANSWER SECTION:
4.4.8.8.in-addr.arpa.   5       IN      PTR     dns.google.

;; Query time: 51 msec
;; SERVER: 192.168.134.2#53(192.168.134.2) (UDP)
;; WHEN: Mon Feb 20 12:46:22 EST 2023
;; MSG SIZE  rcvd: 73

$ dig -x 8.8.8.8

; <<>> DiG 9.18.1-1-Debian <<>> -x 8.8.8.8
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 46722
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; MBZ: 0x0005, udp: 1280
;; QUESTION SECTION:
;8.8.8.8.in-addr.arpa.          IN      PTR

;; ANSWER SECTION:
8.8.8.8.in-addr.arpa.   5       IN      PTR     dns.google.

;; Query time: 47 msec
;; SERVER: 192.168.134.2#53(192.168.134.2) (UDP)
;; WHEN: Mon Feb 20 12:46:30 EST 2023
;; MSG SIZE  rcvd: 73
```