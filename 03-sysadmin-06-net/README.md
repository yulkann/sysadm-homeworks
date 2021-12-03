# Домашнее задание к занятию "3.6. Компьютерные сети, лекция 1"

1. Работа c HTTP через телнет.
- Подключитесь утилитой телнет к сайту stackoverflow.com
`telnet stackoverflow.com 80`
- отправьте HTTP запрос
```bash
GET /questions HTTP/1.0
HOST: stackoverflow.com
[press enter]
[press enter]
```
- В ответе укажите полученный HTTP код, что он означает?

             vagrant@vagrant:~$ telnet stackoverflow.com 80
            Trying 151.101.1.69...
            Connected to stackoverflow.com.
            Escape character is '^]'.
            GET /questions HTTP/1.0
            HOST: stackoverflow.com

            HTTP/1.1 301 Moved Permanently
            cache-control: no-cache, no-store, must-revalidate
            location: https://stackoverflow.com/questions
            x-request-guid: b1172640-0e61-4cfd-a2f9-0c274ea9c776
            feature-policy: microphone 'none'; speaker 'none'
            content-security-policy: upgrade-insecure-requests; frame-ancestors 'self' https://stackexchange.com
            Accept-Ranges: bytes
            Date: Fri, 03 Dec 2021 17:45:57 GMT
            Via: 1.1 varnish
            Connection: close
            X-Served-By: cache-hel1410030-HEL
            X-Cache: MISS
            X-Cache-Hits: 0
            X-Timer: S1638553557.131122,VS0,VE110
            Vary: Fastly-SSL
            X-DNS-Prefetch-Control: off
            Set-Cookie: prov=3788e2e6-4788-ee37-5407-be81c286e2f7; domain=.stackoverflow.com; expires=Fri, 01-Jan-2055 00:00:00 GMT; path=/; HttpOnly

            Connection closed by foreign host.
            
            Код состояния HTTP 301 или Moved Permanently (с англ. — «Перемещено навсегда») 
            — стандартный код ответа HTTP, получаемый в ответ от сервера в ситуации, 
            когда запрошенный ресурс был на постоянной основе перемещён в новое месторасположение, 
            и указывающий на то, что текущие ссылки, использующие данный URL, должны быть обновлены. 
            Адрес нового месторасположения ресурса указывается 
            в поле Location получаемого в ответ заголовка пакета протокола HTTP.

2. Повторите задание 1 в браузере, используя консоль разработчика F12.
- откройте вкладку `Network`
- отправьте запрос http://stackoverflow.com
- найдите первый ответ HTTP сервера, откройте вкладку `Headers`
- укажите в ответе полученный HTTP код. 

            HTTP/2 200 OK
            
- проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?

            самый первый запрос обрабатывался дольше всего
            
- приложите скриншот консоли браузера в ответ.

![изображение](https://user-images.githubusercontent.com/91043924/144650787-c2ffa7df-2c13-47f0-a69f-aafeaa653b9f.png)

![изображение](https://user-images.githubusercontent.com/91043924/144650554-8c46b559-84aa-49e1-9b6c-25ee61851a82.png)

3. Какой IP адрес у вас в интернете?
           
           5.164.212.115
           
5. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой `whois`
              
              route:          5.164.212.0/22
              origin:         AS42682
              org:            ORG-CNN1-RIPE
              descr:          CJSC "ER-Telecom Holding" Nizhny Novgorod branch
              
7. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой `traceroute`
  
                1     4 ms     2 ms     2 ms  192.168.3.1
                2     4 ms     3 ms     3 ms  dynamicip-95-79-247-252.pppoe.nn.ertelecom.ru [95.79.247.252]
                3     5 ms     4 ms     8 ms  lag-2-435.bgw01.nn.ertelecom.ru [91.144.185.82]
                4    12 ms    12 ms    15 ms  72.14.215.165
                5    12 ms    16 ms    11 ms  72.14.215.166
                6    13 ms    13 ms    10 ms  142.251.53.69
                7    10 ms     *       20 ms  108.170.250.113
                8    25 ms    41 ms     *     142.251.49.158
                9    25 ms    25 ms    24 ms  216.239.43.20
               10    25 ms    26 ms    26 ms  216.239.54.201
               11     *        *        *     Превышен интервал ожидания для запроса.
               12     *        *        *     Превышен интервал ожидания для запроса.
               13     *        *        *     Превышен интервал ожидания для запроса.
               14     *        *        *     Превышен интервал ожидания для запроса.
               15     *        *        *     Превышен интервал ожидания для запроса.
               16     *        *        *     Превышен интервал ожидания для запроса.
               17     *        *        *     Превышен интервал ожидания для запроса.
               18     *        *        *     Превышен интервал ожидания для запроса.
               19     *        *        *     Превышен интервал ожидания для запроса.
               20    25 ms    24 ms     *     dns.google [8.8.8.8]
               21     *       26 ms    26 ms  dns.google [8.8.8.8]

               AS42682
               AS15169

9. Повторите задание 5 в утилите `mtr`. На каком участке наибольшая задержка - delay?
              
              vagrant (10.0.2.15)                                                                                                                                                                     2021-12-03T18:10:26+0000
              Keys:  Help   Display mode   Restart statistics   Order of fields   quit
                                                                                                                                                                                      Packets               Pings
               Host                                                                                                                                                                 Loss%   Snt   Last   Avg  Best  Wrst StDev
               1. _gateway                                                                                                                                                           0.0%    36    0.5   0.4   0.2   0.8   0.1
               2. 192.168.3.1                                                                                                                                                        0.0%    36    5.7   4.5   2.5  19.3   3.0
               3. dynamicip-95-79-247-252.pppoe.nn.ertelecom.ru                                                                                                                      0.0%    36    4.8   5.6   4.0  19.4   2.6
               4. lag-2-435.bgw01.nn.ertelecom.ru                                                                                                                                    0.0%    36   13.3   7.1   4.4  39.0   6.2
               5. 72.14.215.165                                                                                                                                                      0.0%    36   13.7  15.1  11.1  38.0   5.5
               6. 72.14.215.166                                                                                                                                                      0.0%    36   15.7  14.0  12.2  18.5   1.7
               7. 142.251.53.69                                                                                                                                                      0.0%    36   11.6  12.2  10.9  15.5   1.2
               8. 108.170.250.113                                                                                                                                                   22.9%    36   75.1  16.1  10.9  75.1  12.9
               9. 142.251.49.158                                                                                                                                                    38.9%    36   28.8  27.4  25.5  30.9   1.8
              10. 216.239.43.20                                                                                                                                                      0.0%    36   26.3  30.3  24.9  61.9   7.3
              11. 216.239.54.201                                                                                                                                                     0.0%    36   26.3  27.3  25.8  30.8   1.4
              12. (waiting for reply)
              13. (waiting for reply)
              14. (waiting for reply)
              15. (waiting for reply)
              16. (waiting for reply)
              17. (waiting for reply)
              18. (waiting for reply)
              19. (waiting for reply)
              20. dns.google                                                                                                                                                        88.2%    35   30.2  26.9  25.6  30.2   2.2

              самая большая задержка в среднем  на  216.239.43.20      

10. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? воспользуйтесь утилитой `dig`
           
           dns.google.             636     IN      A       8.8.4.4
            dns.google.             636     IN      A       8.8.8.8
            
11. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? воспользуйтесь утилитой `dig`

          4.4.8.8.in-addr.arpa.   899     IN      PTR     dns.google.
          8.8.8.8.in-addr.arpa.   363     IN      PTR     dns.google.




