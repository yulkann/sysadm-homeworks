# Домашнее задание к занятию "3.4. Операционные системы, лекция 2"

1. На лекции мы познакомились с [node_exporter](https://github.com/prometheus/node_exporter/releases). В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой [unit-файл](https://www.freedesktop.org/software/systemd/man/systemd.service.html) для node_exporter:

    * поместите его в автозагрузку,
    * предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на `systemctl cat cron`),
    * удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.

                  vagrant@vagrant:/tmp/node_exporter-1.0.1.linux-amd64$ sudo systemctl daemon-reload
                  vagrant@vagrant:/tmp/node_exporter-1.0.1.linux-amd64$ sudo systemctl enable --now node_exporter
                  vagrant@vagrant:/tmp/node_exporter-1.0.1.linux-amd64$ sudo systemctl status node_exporter
                  ● node_exporter.service - Prometheus Node Exporter
                       Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
                       Active: active (running) since Sat 2021-11-13 21:01:18 UTC; 13s ago
           
           после рестарта
           
                  vagrant@vagrant:/tmp/node_exporter-1.0.1.linux-amd64$ sudo systemctl restart node_exporter
                  vagrant@vagrant:/tmp/node_exporter-1.0.1.linux-amd64$ sudo systemctl status node_exporter
                  ● node_exporter.service - Prometheus Node Exporter
                       Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
                       Active: active (running) since Sat 2021-11-13 21:03:54 UTC; 2s ago

           после перезагрузки
           
                  vagrant@vagrant:~$ sudo systemctl status node_exporter
                  ● node_exporter.service - Prometheus Node Exporter
                       Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
                       Active: active (running) since Sat 2021-11-13 21:06:41 UTC; 26s ago
              
            
            [Unit]
            Description=Prometheus Node Exporter
            Wants=network-online.target
            After=network-online.target

            [Service]
            User=node_exporter
            Group=node_exporter
            Type=simple
            EnvironmentFile=/etc/default/node_exporter
            ExecStart=/usr/local/bin/node_exporter

            [Install]
            WantedBy=multi-user.target
            

1. Ознакомьтесь с опциями node_exporter и выводом `/metrics` по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.
             
                  --collector.cpu.info      Enables metric cpu_info
                  --collector.perf.cpus=""  List of CPUs from which perf metrics should be
                  --collector.cpu           Enable the cpu collector (default: enabled).
                  --collector.cpufreq       Enable the cpufreq collector (default: enabled).
                  --collector.meminfo       Enable the meminfo collector (default: enabled).
                  --collector.meminfo_numa  Enable the meminfo_numa collector (default:
                  --collector.diskstats.ignored-devices="^(ram|loop|fd|(h|s|v|xv)d[a-z]|nvme\\d+n\\d+p)\\d+$"
                                            Regexp of devices to ignore for diskstats.
                  --collector.diskstats     Enable the diskstats collector (default:

               --collector.netclass.ignored-devices="^$"
                                            Regexp of net devices to ignore for netclass
                  --collector.netdev.device-blacklist=COLLECTOR.NETDEV.DEVICE-BLACKLIST
                                            Regexp of net devices to blacklist (mutually
                  --collector.netdev.device-whitelist=COLLECTOR.NETDEV.DEVICE-WHITELIST
                                            Regexp of net devices to whitelist (mutually
                 --collector.netstat.fields="^(.*_(InErrors|InErrs)|Ip_Forwarding|Ip(6|Ext)_(InOctets|OutOctets)|Icmp6?_(InMsgs|OutMsgs)|TcpExt_(Listen.*|Syncookies.*|TCPSynRetrans)|Tcp_(ActiveOpens|InSegs|OutSegs|PassiveOpens|RetransSegs|CurrEstab)|Udp6?_(InDatagrams|OutDatagrams|NoPorts|RcvbufErrors|SndbufErrors))$"
                                            Regexp of fields to return for netstat
                  --collector.netclass      Enable the netclass collector (default:
                  --collector.netdev        Enable the netdev collector (default: enabled).
                  --collector.netstat       Enable the netstat collector (default: enabled).
      
3. Установите в свою виртуальную машину [Netdata](https://github.com/netdata/netdata). Воспользуйтесь [готовыми пакетами](https://packagecloud.io/netdata/netdata/install) для установки (`sudo apt install -y netdata`). После успешной установки:
    * в конфигурационном файле `/etc/netdata/netdata.conf` в секции [web] замените значение с localhost на `bind to = 0.0.0.0`,
    * добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте `vagrant reload`:

    ```bash
    config.vm.network "forwarded_port", guest: 19999, host: 19999
    ```

    После успешной перезагрузки в браузере *на своем ПК* (не в виртуальной машине) вы должны суметь зайти на `localhost:19999`. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам.

      ![изображение](https://user-images.githubusercontent.com/91043924/141658230-ef4cade3-0613-4e9f-b89a-f00580ce516d.png)

1. Можно ли по выводу `dmesg` понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?

                  Думаю да:
                    vagrant@vagrant:~$ cat /var/log/dmesg | grep virtual
                   [    0.002790] kernel: CPU MTRRs all blank - `virtualized system`.
                   [    0.115339] kernel: Booting paravirtualized kernel on KVM
                   [    0.277065] kernel: Performance Events: PMU not available due to virtualization, using software events only.
                   [    3.226381] systemd[1]: Detected virtualization oracle.


3. Как настроен sysctl `fs.nr_open` на системе по-умолчанию? Узнайте, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (`ulimit --help`)?
   
                     fs.nr_open = 1048576 
                     максимальное количество файловых дескрипторов
                     vagrant@vagrant:/tmp$ ulimit -Hn
                     1048576


3. Запустите любой долгоживущий процесс (не `ls`, который отработает мгновенно, а, например, `sleep 1h`) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через `nsenter`. Для простоты работайте в данном задании под root (`sudo -i`). Под обычным пользователем требуются дополнительные опции (`--map-root-user`) и т.д.

                     root@vagrant:/# ps aux
                     USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
                     root           1  0.0  0.0   8076   592 pts/2    S+   22:14   0:00 sleep 1h
                     root           2  0.0  0.3   9836  4012 pts/3    S    22:16   0:00 -bash
                     root          11  0.0  0.3  11492  3328 pts/3    R+   22:16   0:00 ps aux
      
3. Найдите информацию о том, что такое `:(){ :|:& };:`. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (**это важно, поведение в других ОС не проверялось**). Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов `dmesg` расскажет, какой механизм помог автоматической стабилизации. Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?
         
                     fork-бомба — вредоносная или ошибочно написанная программа, бесконечно создающая свои копии (системным вызовом fork()), которые обычно также начинают создавать свои копии и т. д.

            Выполнение такой программы может вызывать большую нагрузку вычислительной системы или даже отказ в обслуживании вследствие нехватки системных ресурсов (дескрипторов процессов, памяти, процессорного времени), что и является целью. 
         
                     vagrant@vagrant:~$ :(){ :|:& };:
                     [1] 58964
                     vagrant@vagrant:~$ -bash: fork: retry: Resource temporarily unavailable
                     -bash: fork: retry: Resource temporarily unavailable
                     -bash: fork: retry: Resource temporarily unavailable
                     -bash: fork: retry: Resource temporarily unavailable

         [ 1101.987163] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-3.scope
         
                  Один из способов предотвращения негативных последствий работы fork-бомбы — принудительное ограничение количества процессов, которые пользователь может запустить одновременно.
                  ulimit -u 20
                  
