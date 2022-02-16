1) Узнайте о sparse (разряженных) файлах.

Разряжённый файл — файл, в котором последовательности нулевых байтов заменены на информацию об этих последовательностях. Нужны для экономии места на диске.

2) Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?

Не могут, так как жесткие ссылки ссылаются на одну inode в которой и содержится информация о правах и владельце.

3) Сделайте vagrant destroy на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:
  
  ```commandline
  Vagrant.configure("2") do |config|
    config.vm.box = "bento/ubuntu-20.04"
    config.vm.provider :virtualbox do |vb|
      lvm_experiments_disk0_path = "/tmp/lvm_experiments_disk0.vmdk"
      lvm_experiments_disk1_path = "/tmp/lvm_experiments_disk1.vmdk"
      vb.customize ['createmedium', '--filename', lvm_experiments_disk0_path, '--size', 2560]
      vb.customize ['createmedium', '--filename', lvm_experiments_disk1_path, '--size', 2560]
      vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk0_path]
      vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk1_path]
    end
  end
  ```
  
Данная конфигурация создаст новую виртуальную машину с двумя дополнительными неразмеченными дисками по 2.5 Гб.

  ```commandline
  vagrant@vagrant:~$ lsblk
  NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
  loop0                       7:0    0 55.4M  1 loop /snap/core18/2128
  loop1                       7:1    0 32.3M  1 loop /snap/snapd/12704
  loop2                       7:2    0 70.3M  1 loop /snap/lxd/21029
  loop3                       7:3    0 55.5M  1 loop /snap/core18/2284
  loop4                       7:4    0 43.6M  1 loop /snap/snapd/14978
  sda                         8:0    0   64G  0 disk
  ├─sda1                      8:1    0    1M  0 part
  ├─sda2                      8:2    0    1G  0 part /boot
  └─sda3                      8:3    0   63G  0 part
    └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm  /
  sdb                         8:16   0  2.5G  0 disk
  sdc                         8:32   0  2.5G  0 disk
  ```
  
4) Используя fdisk, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.

Отображение дисковой системы после изменений:

```commandline
    vagrant@vagrant:~$ lsblk
    NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    loop0                       7:0    0 55.4M  1 loop /snap/core18/2128
    loop2                       7:2    0 70.3M  1 loop /snap/lxd/21029
    loop3                       7:3    0 55.5M  1 loop /snap/core18/2284
    loop4                       7:4    0 43.6M  1 loop /snap/snapd/14978
    loop5                       7:5    0 61.9M  1 loop /snap/core20/1328
    loop6                       7:6    0 67.2M  1 loop /snap/lxd/21835
    sda                         8:0    0   64G  0 disk
    ├─sda1                      8:1    0    1M  0 part
    ├─sda2                      8:2    0    1G  0 part /boot
    └─sda3                      8:3    0   63G  0 part
      └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm  /
    sdb                         8:16   0  2.5G  0 disk
    ├─sdb1                      8:17   0    2G  0 part
    └─sdb2                      8:18   0  511M  0 part
    sdc                         8:32   0  2.5G  0 disk
```
    
5) Используя sfdisk, перенесите данную таблицу разделов на второй диск.

     ```commandline
     vagrant@vagrant:~$ sudo sfdisk -d /dev/sdb| sudo sfdisk /dev/sdc
     Checking that no-one is using this disk right now ... OK

     Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
     Disk model: VBOX HARDDISK
     Units: sectors of 1 * 512 = 512 bytes
     Sector size (logical/physical): 512 bytes / 512 bytes
     I/O size (minimum/optimal): 512 bytes / 512 bytes

     >>> Script header accepted.
     >>> Script header accepted.
     >>> Script header accepted.
     >>> Script header accepted.
     >>> Created a new DOS disklabel with disk identifier 0x8faf5c0d.
     /dev/sdc1: Created a new partition 1 of type 'Linux' and of size 2 GiB.
     /dev/sdc2: Created a new partition 2 of type 'Linux' and of size 511 MiB.
     /dev/sdc3: Done.

     New situation:
     Disklabel type: dos
     Disk identifier: 0x8faf5c0d

     Device     Boot   Start     End Sectors  Size Id Type
     /dev/sdc1          2048 4196351 4194304    2G 83 Linux
     /dev/sdc2       4196352 5242879 1046528  511M 83 Linux

     The partition table has been altered.
     Calling ioctl() to re-read partition table.
     Syncing disks.
     ```
6) Соберите mdadm RAID1 на паре разделов 2 Гб.

Соберите mdadm RAID0 на второй паре маленьких разделов.

Создайте 2 независимых PV на получившихся md-устройствах.

Создайте общую volume-group на этих двух PV.

Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.

Создайте mkfs.ext4 ФС на получившемся LV.

Смонтируйте этот раздел в любую директорию, например, /tmp/new.

Поместите туда тестовый файл, например wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.

Прикрепите вывод lsblk.

Протестируйте целостность файла:

root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0
Используя pvmove, переместите содержимое PV с RAID0 на RAID1.

Сделайте --fail на устройство в вашем RAID1 md.

Подтвердите выводом dmesg, что RAID1 работает в деградированном состоянии.

Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:

root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0
20) Погасите тестовый хост, vagrant destroy.
