1) На лекции мы познакомились с node_exporter. В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой unit-файл для node_exporter:
-поместите его в автозагрузку,
-предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на systemctl cat cron),
-удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.

Ответ:

Скачан архив:
     
     сd tmp     
     wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
     tar xvfz node_exporter-0.18.1.linux-amd64.tar.gz
     cd node_exporter-0.18.1.linux-amd64

Перенесен в папку /usr/local/bin/:

     mv node_exporter /usr/local/bin/
     
Создан конфигурационный файл node_exporter в /etc/sysconfig/ 

     cat <<EOF > /etc/sysconfig/node_exporter
     OPTIONS=""
     EOF

Создаем systemd сервис
     сat <<EOF > /usr/lib/systemd/system/node_exporter.service

     [Unit]
     Description=Prometheus Node exporter for machine metrics
     Documentation=https://github.com/prometheus/node_exporter

     [Service]
     Restart=always
     User=root
     EnvironmentFile=/etc/sysconfig/node_exporter
     ExecStart=/usr/local/bin/node_exporter \$OPTIONS
     ExecReload=/bin/kill -HUP \$MAINPID
     TimeoutStopSec=20s
     SendSIGKILL=no

     [Install]
     WantedBy=multi-user.target
     EOF

Добавляем в автозагрузку:
     
     systemctl enable node_exporter
     systemctl daemon-reload
     systemctl start node_exporter

Проверка статуса:
     
     root@vagrant:~# systemctl status node_exporter
     ● node_exporter.service - Prometheus Node exporter for machine metrics
          Loaded: loaded (/lib/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
          Active: active (running) since Thu 2022-02-10 17:12:21 UTC; 2h 8min ago
            Docs: https://github.com/prometheus/node_exporter
        Main PID: 607 (node_exporter)
           Tasks: 5 (limit: 1071)
          Memory: 6.6M
          CGroup: /system.slice/node_exporter.service
                  └─607 /usr/local/bin/node_exporter --log.level=info

     Feb 10 17:12:21 vagrant node_exporter[607]: level=info ts=2022-02-10T17:12:21.664Z caller=node_exporter.go:115 collecto>
     Feb 10 17:12:21 vagrant node_exporter[607]: level=info ts=2022-02-10T17:12:21.664Z caller=node_exporter.go:115 collecto>
     Feb 10 17:12:21 vagrant node_exporter[607]: level=info ts=2022-02-10T17:12:21.664Z caller=node_exporter.go:115 collecto>
     Feb 10 17:12:21 vagrant node_exporter[607]: level=info ts=2022-02-10T17:12:21.664Z caller=node_exporter.go:115 collecto>
     Feb 10 17:12:21 vagrant node_exporter[607]: level=info ts=2022-02-10T17:12:21.664Z caller=node_exporter.go:115 collecto>
     Feb 10 17:12:21 vagrant node_exporter[607]: level=info ts=2022-02-10T17:12:21.664Z caller=node_exporter.go:115 collecto>
     Feb 10 17:12:21 vagrant node_exporter[607]: level=info ts=2022-02-10T17:12:21.664Z caller=node_exporter.go:115 collecto>
     Feb 10 17:12:21 vagrant node_exporter[607]: level=info ts=2022-02-10T17:12:21.664Z caller=node_exporter.go:115 collecto>
     Feb 10 17:12:21 vagrant node_exporter[607]: level=info ts=2022-02-10T17:12:21.665Z caller=node_exporter.go:199 msg="Lis>
     Feb 10 17:12:21 vagrant node_exporter[607]: level=info ts=2022-02-10T17:12:21.665Z caller=tls_config.go:191 msg="TLS is>
     
передается параметр `--log.level=info`
Работоспособность можно проверить по адресу http://localhost:9100/metrics

2) Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.
     
Ответ:     
Для просмотра метрик можно использовать команду `curl localhost:9100/metrics` либо по адресу http://localhost:9100/metrics
     
Для CPU:

    node_cpu_seconds_total{cpu="0",mode="idle"} 
    node_cpu_seconds_total{cpu="0",mode="system"} 
    node_cpu_seconds_total{cpu="0",mode="user"}
    node_cpu_seconds_total{cpu="1",mode="idle"} 
    node_cpu_seconds_total{cpu="1",mode="system"} 
    node_cpu_seconds_total{cpu="1",mode="user"}
    process_cpu_seconds_total
Для памяти:

    node_memory_MemAvailable_bytes 
    node_memory_MemFree_bytes
Для диска:

    node_disk_io_time_seconds_total{device="sda"} 
    node_disk_read_time_seconds_total{device="sda"} 
    node_disk_write_time_seconds_total{device="sda"}
Для сети:

    node_network_receive_errs_total{device="eth0"} 
    node_network_receive_bytes_total{device="eth0"} 
    node_network_transmit_bytes_total{device="eth0"}
    node_network_transmit_errs_total{device="eth0"}
     
3) Установите в свою виртуальную машину Netdata. Воспользуйтесь готовыми пакетами для установки (sudo apt install -y netdata). После успешной установки:
в конфигурационном файле /etc/netdata/netdata.conf в секции [web] замените значение с localhost на bind to = 0.0.0.0,
добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте vagrant reload:
config.vm.network "forwarded_port", guest: 19999, host: 19999
После успешной перезагрузки в браузере на своем ПК (не в виртуальной машине) вы должны суметь зайти на localhost:19999. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам.
     
 С данным заданием возникли сложности.
     
 Netdata установлена и запущена, но в браузере открыть по адресу http://localhost:19999 не  удается.
 
          root@vagrant:~# systemctl status netdata
     ● netdata.service - Real time performance monitoring
          Loaded: loaded (/lib/systemd/system/netdata.service; enabled; vendor preset: enabled)
          Active: active (running) since Thu 2022-02-10 21:24:48 UTC; 1h 1min ago
        Main PID: 20534 (netdata)
           Tasks: 61 (limit: 1071)
          Memory: 112.0M
          CGroup: /system.slice/netdata.service
                  ├─20534 /usr/sbin/netdata -D
                  ├─20546 /usr/sbin/netdata --special-spawn-server
                  ├─20705 /usr/libexec/netdata/plugins.d/apps.plugin 1
                  ├─20707 /usr/libexec/netdata/plugins.d/nfacct.plugin 1
                  ├─20708 /usr/libexec/netdata/plugins.d/go.d.plugin 1
                  ├─20713 /usr/libexec/netdata/plugins.d/ebpf.plugin 1
                  └─42686 bash /usr/libexec/netdata/plugins.d/tc-qos-helper.sh 1

     Feb 10 21:24:48 vagrant netdata[20534]: 2022-02-10 21:24:48: netdata INFO  : MAIN : CONFIG: cannot load cloud config '/var/lib/netdata/cloud.d/cloud.conf'. Running with internal defaults.
     Feb 10 21:24:48 vagrant netdata[20534]: 2022-02-10 21:24:48: netdata INFO  : MAIN : Found 0 legacy dbengines, setting multidb diskspace to 256MB
     Feb 10 21:24:48 vagrant netdata[20534]: 2022-02-10 21:24:48: netdata INFO  : MAIN : Created file '/var/lib/netdata/dbengine_multihost_size' to store the computed value
     Feb 10 21:24:48 vagrant netdata[20534]: Found 0 legacy dbengines, setting multidb diskspace to 256MB
     Feb 10 21:24:48 vagrant netdata[20534]: Created file '/var/lib/netdata/dbengine_multihost_size' to store the computed value
     Feb 10 21:24:49 vagrant ebpf.plugin[20713]: Does not have a configuration file inside `/etc/netdata/ebpf.d.conf. It will try to load stock file.
     Feb 10 21:24:49 vagrant ebpf.plugin[20713]: Name resolution is disabled, collector will not parser "hostnames" list.
     Feb 10 21:24:49 vagrant ebpf.plugin[20713]: The network value of CIDR 127.0.0.1/8 was updated for 127.0.0.0 .
     Feb 10 21:24:49 vagrant ebpf.plugin[20713]: PROCFILE: Cannot open file '/etc/netdata/apps_groups.conf'
     Feb 10 21:24:49 vagrant ebpf.plugin[20713]: Cannot read process groups configuration file '/etc/netdata/apps_groups.conf'. Will try '/usr/lib/netdata/conf.d/apps_groups.conf'

  Порт работает на прием
     
          root@vagrant:~# ss -tnlp | grep :19999
     LISTEN    0         4096               0.0.0.0:19999            0.0.0.0:*        users:(("netdata",pid=20534,fd=5))
     
4) Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?

Ответ:
Да, можно понять.
     
Консоль:
     
     vagrant@vagrant:~$ dmesg |grep virt
     [    0.031304] CPU MTRRs all blank - virtualized system.
     [    0.173972] Booting paravirtualized kernel on KVM
     [    0.390287] Performance Events: PMU not available due to virtualization, using software events only.
     [    3.213111] systemd[1]: Detected virtualization oracle.

5) Как настроен sysctl fs.nr_open на системе по-умолчанию? Узнайте, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (ulimit --help)?

Ответ:
     
     vagrant@vagrant:~$ sysctl fs.nr_open
     fs.nr_open = 1048576
     
Этот параметр ограничивает количество одновременно открытых дескрипторов. По умолчанию - 1048576
Максимальное количество ограничивается командой:
     
     ``ulimit -h [limit]`

6) Запустите любой долгоживущий процесс (не ls, который отработает мгновенно, а, например, sleep 1h) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через nsenter. Для простоты работайте в данном задании под root (sudo -i). Под обычным пользователем требуются дополнительные опции (--map-root-user) и т.д.
     
Ответ:
     
     root@vagrant:~# unshare -f --pid --mount-proc sleep 10h
     ^Z
     [1]+  Stopped                 unshare -f --pid --mount-proc sleep 10h
     root@vagrant:~# ps aux | grep slee
     root        3507  0.0  0.0   5480   584 pts/0    T    18:23   0:00 unshare -f --pid --mount-proc sleep 10h
     root        3508  0.0  0.0   5476   532 pts/0    S    18:23   0:00 sleep 10h
     root        3514  0.0  0.0   6300   736 pts/0    S+   18:24   0:00 grep --color=auto slee
     root@vagrant:~# nsenter --target 3508 --pid --mount
     root@vagrant:/# ps aux
     USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
     root           1  0.0  0.0   5476   532 pts/0    S    18:23   0:00 sleep 10h
     root           2  0.0  0.3   7236  3976 pts/0    S    18:24   0:00 -bash
     root          13  0.0  0.3   8892  3120 pts/0    R+   18:25   0:00 ps aux
     root@vagrant:/#


7) Найдите информацию о том, что такое :(){ :|:& };:. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (это важно, поведение в других ОС не проверялось). Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов dmesg расскажет, какой механизм помог автоматической стабилизации. Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?

Ответ:
     
`:(){ :|:& };:` - создает функцию : которая уходит в фон и создает саму себя снова, получается бесконечная рекурсия с порождением все новых и новых процессов


Процесс который остановил рекурсию
     
     [ 5484.569926] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-3.scope

Посмотреть количество всех лимитов можно через команду `ulimit -a`
     
     root@vagrant:~# ulimit -a
     core file size          (blocks, -c) 0
     data seg size           (kbytes, -d) unlimited
     scheduling priority             (-e) 0
     file size               (blocks, -f) unlimited
     pending signals                 (-i) 3571
     max locked memory       (kbytes, -l) 65536
     max memory size         (kbytes, -m) unlimited
     open files                      (-n) 1024
     pipe size            (512 bytes, -p) 8
     POSIX message queues     (bytes, -q) 819200
     real-time priority              (-r) 0
     stack size              (kbytes, -s) 8192
     cpu time               (seconds, -t) unlimited
     max user processes              (-u) 3571
     virtual memory          (kbytes, -v) unlimited
     file locks                      (-x) unlimited

 По умолчанию установлено 3571 активных процессов
