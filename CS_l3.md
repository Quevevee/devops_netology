# Домашнее задание к лекции "Компьютерные сети, лекция 3"

### 1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP

````bash
$ telnet route-views.routeviews.org
Trying 128.223.51.103...
Connected to route-views.routeviews.org.
Escape character is '^]'.
C
**********************************************************************

                    RouteViews BGP Route Viewer
                    route-views.routeviews.org

 route views data is archived on http://archive.routeviews.org

 This hardware is part of a grant by the NSF.
 Please contact help@routeviews.org if you have questions, or
 if you wish to contribute your view.

 This router has views of full routing tables from several ASes.
 The list of peers is located at http://www.routeviews.org/peers
 in route-views.oregon-ix.net.txt

 NOTE: The hardware was upgraded in August 2014.  If you are seeing
 the error message, "no default Kerberos realm", you may want to
 in Mac OS X add "default unset autologin" to your ~/.telnetrc

 To login, use the username "rviews".

 **********************************************************************

User Access Verification

Username: rviews 
route-views>show ip route 46.158.141.162   
Routing entry for 46.158.0.0/16
  Known via "bgp 6447", distance 20, metric 0
  Tag 3303, type external
  Last update from 217.192.89.50 5w1d ago
  Routing Descriptor Blocks:
  * 217.192.89.50, from 217.192.89.50, 5w1d ago
      Route metric is 0, traffic share count is 1
      AS Hops 2
      Route tag 3303
      MPLS label: none
route-views>show bgp 46.158.141.162 
BGP routing table entry for 46.158.0.0/16, version 2653813311
Paths: (20 available, best #20, table default)
  Not advertised to any peer
  Refresh Epoch 1
  3267 12389
    194.85.40.15 from 194.85.40.15 (185.141.126.1)
      Origin IGP, metric 0, localpref 100, valid, external
      path 7FE04B4930B0 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  8283 1299 12389
    94.142.247.3 from 94.142.247.3 (94.142.247.3)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 1299:30000 8283:1 8283:101 8283:102
      unknown transitive attribute: flag 0xE0 type 0x20 length 0x24
        value 0000 205B 0000 0000 0000 0001 0000 205B
              0000 0005 0000 0001 0000 205B 0000 0005
              0000 0002 
      path 7FE173442B08 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  7018 3356 12389
    12.0.1.63 from 12.0.1.63 (12.0.1.63)
      Origin IGP, localpref 100, valid, external
      Community: 7018:5000 7018:37232
      path 7FE10B4E3EA8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20912 3257 1299 12389
    212.66.96.126 from 212.66.96.126 (212.66.96.126)
      Origin IGP, localpref 100, valid, external
      Community: 3257:8066 3257:30055 3257:50001 3257:53900 3257:53902 20912:65004
      path 7FE1424CBB90 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  49788 12552 12389
    91.218.184.60 from 91.218.184.60 (91.218.184.60)
      Origin IGP, localpref 100, valid, external
      Community: 12552:12000 12552:12100 12552:12101 12552:22000
      Extended Community: 0x43:100:0
      path 7FE1CE4A60C8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3333 1103 12389
    193.0.0.56 from 193.0.0.56 (193.0.0.56)
      Origin IGP, localpref 100, valid, external
      path 7FE1525932F0 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  6939 12389
    64.71.137.241 from 64.71.137.241 (216.218.253.53)
      Origin IGP, localpref 100, valid, external
      path 7FE09B3A0478 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  19214 174 12389
    208.74.64.40 from 208.74.64.40 (208.74.64.40)
      Origin IGP, localpref 100, valid, external
      Community: 174:21101 174:22005
      path 7FE0F0CDA2A8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3257 3356 12389
    89.149.178.10 from 89.149.178.10 (213.200.83.26)
      Origin IGP, metric 10, localpref 100, valid, external
      Community: 3257:8794 3257:30043 3257:50001 3257:54900 3257:54901
      path 7FE0E0A35D38 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3561 3910 3356 12389
    206.24.210.80 from 206.24.210.80 (206.24.210.80)
      Origin IGP, localpref 100, valid, external
      path 7FE12F974C30 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3549 3356 12389
    208.51.134.254 from 208.51.134.254 (67.16.168.191)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:901 3356:2065 3549:2581 3549:30840
      path 7FE02808C6A8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  4901 6079 3356 12389
    162.250.137.254 from 162.250.137.254 (162.250.137.254)
      Origin IGP, localpref 100, valid, external
      Community: 65000:10100 65000:10300 65000:10400
      path 7FE0DE83B5B0 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  101 3356 12389
    209.124.176.223 from 209.124.176.223 (209.124.176.223)
      Origin IGP, localpref 100, valid, external
      Community: 101:20100 101:20110 101:22100 3356:2 3356:22 3356:100 3356:123 3356:501 3356:901 3356:2065
      Extended Community: RT:101:22100
      path 7FE02C8A5280 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3356 12389
    4.68.4.46 from 4.68.4.46 (4.69.184.201)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:901 3356:2065
      path 7FE09AFAA630 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  1351 6939 12389
    132.198.255.253 from 132.198.255.253 (132.198.255.253)
      Origin IGP, localpref 100, valid, external
      path 7FE04A0DE250 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  57866 3356 12389
    37.139.139.17 from 37.139.139.17 (37.139.139.17)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:901 3356:2065 57866:100 65100:3356 65103:1 65104:31
      unknown transitive attribute: flag 0xE0 type 0x20 length 0x30
        value 0000 E20A 0000 0064 0000 0D1C 0000 E20A
              0000 0065 0000 0064 0000 E20A 0000 0067
              0000 0001 0000 E20A 0000 0068 0000 001F
              
      path 7FE0D093B4E0 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 2
  2497 12389
    202.232.0.2 from 202.232.0.2 (58.138.96.254)
      Origin IGP, localpref 100, valid, external
      path 7FE18C674608 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20130 6939 12389
    140.192.8.16 from 140.192.8.16 (140.192.8.16)
      Origin IGP, localpref 100, valid, external
      path 7FE11366BBF8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  852 3356 12389
    154.11.12.212 from 154.11.12.212 (96.1.209.43)
      Origin IGP, metric 0, localpref 100, valid, external
      path 7FE0F0375608 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3303 12389
    217.192.89.50 from 217.192.89.50 (138.187.128.158)
      Origin IGP, localpref 100, valid, external, best
      Community: 3303:1004 3303:1006 3303:1030 3303:3056
      path 7FE105044498 RPKI State valid
      rx pathid: 0, tx pathid: 0x0
route-views>Connection closed by foreign host.
````

### 2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.  

```bash
$ sudo ip link add dummy0 type dummy
$ sudo ip addr add 10.0.10.1/24 dev dummy0
$ sudo ip link set dummy0 up
$ sudo ip route add to 10.10.0.0/16 via 10.0.10.1
$ sudo ip route add to 10.12.0.0/16 via 10.0.10.1
$ ip route
default via 192.168.145.2 dev eth0 proto dhcp metric 100 
192.168.145.0/24 dev eth0 proto kernel scope link src 192.168.145.128 metric 100 
10.0.10.0/24 dev dummy0 proto kernel scope link src 10.0.10.1 
10.10.0.0/16 via 10.0.10.1 dev dummy0 
10.12.0.0/16 via 10.0.10.1 dev dummy0 
10.78.1.0/24 dev eth0.100 proto kernel scope link src 10.78.1.14 
169.254.0.0/16 dev enp3s0 scope link metric 1000 
192.168.145.0/24 dev eth0 proto kernel scope link src 192.168.145.128 metric 100 
```

### 3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.

```bash
$ sudo netstat -ntlp | grep LISTEN
tcp        0      0 0.0.0.0:7071            0.0.0.0:*               LISTEN      1276/anydesk        
tcp        0      0 0.0.0.0:40783           0.0.0.0:*               LISTEN      1276/anydesk        
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      650/systemd-resolve 
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      163202/cupsd        
tcp6       0      0 :::1716                 :::*                    LISTEN      1662/kdeconnectd    
tcp6       0      0 ::1:631                 :::*                    LISTEN      163202/cupsd 
```  
7071 порт TCP использует anydesk  
53 порт TCP использует systemd  
 
### 4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?

```bash
lodyanyy@lodyanyy:~$ ss -lupn
State			Recv-Q			Send-Q			Local Address:Port			Peer Address:Port			Process                                                  
UNCONN			0				0				127.0.0.53%lo:53			0.0.0.0:*                                                                                
UNCONN          0               0               0.0.0.0:631                 0.0.0.0:*                                                                                
UNCONN          0               0               0.0.0.0:54095				0.0.0.0:*                                                                                
UNCONN          0               0               0.0.0.0:50002               0.0.0.0:*                   users:(("anydesk",pid=160142,fd=23))                    
UNCONN          0               0               0.0.0.0:5353                0.0.0.0:*                                                                                
UNCONN          0               0               [::]:36251					[::]:*                                                                                
UNCONN          0               0               [::]:5353                   [::]:*                                                                                
UNCONN          0               0               *:1716                      *:*                         users:(("kdeconnectd",pid=1662,fd=11))  
```
1716 порт UDP использует kdeconnectd  
50002 порт UDP использует anydesk

### 5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали.

