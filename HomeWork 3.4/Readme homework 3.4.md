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
     
     --Работоспособность можно проверить по адресу http://localhost:9100/metrics

2)Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.

3)Установите в свою виртуальную машину Netdata. Воспользуйтесь готовыми пакетами для установки (sudo apt install -y netdata). После успешной установки:

в конфигурационном файле /etc/netdata/netdata.conf в секции [web] замените значение с localhost на bind to = 0.0.0.0,
добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте vagrant reload:
config.vm.network "forwarded_port", guest: 19999, host: 19999
После успешной перезагрузки в браузере на своем ПК (не в виртуальной машине) вы должны суметь зайти на localhost:19999. Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам.

4)Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?
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
ulimit -h [limit]

6)Запустите любой долгоживущий процесс (не ls, который отработает мгновенно, а, например, sleep 1h) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через nsenter. Для простоты работайте в данном задании под root (sudo -i). Под обычным пользователем требуются дополнительные опции (--map-root-user) и т.д.
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
:(){ :|:& };: - создает функцию : которая уходит в фон и создает саму себя снова, получается бесконечная рекурсия с порождением все новых и новых процессов

Процесс который остановил рекурсию
[ 5484.569926] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-3.scope
Посмотреть количество всех лимитов можно через команду ulimit -a
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
