# Домашнее задание к занятию "3.8. Компьютерные сети, лекция 3"

1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP
```
telnet route-views.routeviews.org
Username: rviews
show ip route x.x.x.x/32
show bgp x.x.x.x/32
```
       route-views>show ip route 5.166.202.254
       Routing entry for 5.166.200.0/22
         Known via "bgp 6447", distance 20, metric 0
         Tag 6939, type external
         Last update from 64.71.137.241 3w3d ago
         Routing Descriptor Blocks:
         * 64.71.137.241, from 64.71.137.241, 3w3d ago
             Route metric is 0, traffic share count is 1
             AS Hops 3
             Route tag 6939
             MPLS label: none
      
        route-views>show bgp | inc 5.166.200.0/22
        N*   5.166.200.0/22   162.251.163.2                          0 53767 174 174 1299 9049 9049 9049 42682 i

2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.
            
            3: dummy0: <BROADCAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc noqueue state UNKNOWN group default qlen 1000
                  link/ether 26:3b:42:76:dc:92 brd ff:ff:ff:ff:ff:ff
                  inet 10.2.2.2/32 brd 10.2.2.2 scope global dummy0
                     valid_lft forever preferred_lft forever
                  inet6 fe80::243b:42ff:fe76:dc92/64 scope link
                     valid_lft forever preferred_lft forever
             
              vagrant@vagrant:~$ sudo  ip route
              default via 10.0.2.2 dev eth0 proto dhcp src 10.0.2.15 metric 100
              10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15
              10.0.2.2 dev eth0 proto dhcp scope link src 10.0.2.15 metric 100
              10.2.2.2 dev dummy0 scope link
              192.168.1.0/24 dev dummy0 proto kernel scope link src 192.168.1.150
                     

3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.

              vagrant@vagrant:~$ sudo lsof -nP -iTCP -sTCP:LISTEN
              COMMAND   PID            USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
              systemd     1            root   35u  IPv4    833      0t0  TCP *:111 (LISTEN)
              systemd     1            root   37u  IPv6    837      0t0  TCP *:111 (LISTEN)
              rpcbind   608            _rpc    4u  IPv4    833      0t0  TCP *:111 (LISTEN)
              rpcbind   608            _rpc    6u  IPv6    837      0t0  TCP *:111 (LISTEN)
              systemd-r 609 systemd-resolve   13u  IPv4  20824      0t0  TCP 127.0.0.53:53 (LISTEN)
              sshd      898            root    3u  IPv4  24977      0t0  TCP *:22 (LISTEN)
              sshd      898            root    4u  IPv6  24979      0t0  TCP *:22 (LISTEN)

4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?

              vagrant@vagrant:~$ ss -pl | grep udp
              udp     UNCONN   0        0                                       127.0.0.53%lo:domain                      0.0.0.0:*
              udp     UNCONN   0        0                                      10.0.2.15%eth0:bootpc                      0.0.0.0:*
              udp     UNCONN   0        0                                             0.0.0.0:sunrpc                      0.0.0.0:*
              udp     UNCONN   0        0                                                [::]:sunrpc                         [::]:*

5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали. 

 ![изображение](https://user-images.githubusercontent.com/91043924/149023201-8ce6dcc5-f3b3-48e1-87e2-10414223ac61.png)


 ---
## Задание для самостоятельной отработки (необязательно к выполнению)

6*. Установите Nginx, настройте в режиме балансировщика TCP или UDP.

7*. Установите bird2, настройте динамический протокол маршрутизации RIP.

8*. Установите Netbox, создайте несколько IP префиксов, используя curl проверьте работу API.



---

