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

      ```
      vagrant@vagrant:~$ sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sd[bc]1
      mdadm: Note: this array has metadata at the start and
          may not be suitable as a boot device.  If you plan to
          store '/boot' on this device please ensure that
          your boot-loader understands md/v1.x metadata, or use
          --metadata=0.90
      Continue creating array? y
      mdadm: Defaulting to version 1.2 metadata
      mdadm: array /dev/md0 started.
      vagrant@vagrant:~$ lsblk
      NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
      loop0                       7:0    0 55.4M  1 loop  /snap/core18/2128
      loop1                       7:1    0 61.9M  1 loop  /snap/core20/1328
      loop2                       7:2    0 61.9M  1 loop  /snap/core20/1361
      loop3                       7:3    0 55.5M  1 loop  /snap/core18/2284
      loop4                       7:4    0 67.2M  1 loop  /snap/lxd/21835
      loop5                       7:5    0 43.6M  1 loop  /snap/snapd/14978
      loop6                       7:6    0 67.9M  1 loop  /snap/lxd/22526
      sda                         8:0    0   64G  0 disk
      ├─sda1                      8:1    0    1M  0 part
      ├─sda2                      8:2    0    1G  0 part  /boot
      └─sda3                      8:3    0   63G  0 part
        └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm   /
      sdb                         8:16   0  2.5G  0 disk
      ├─sdb1                      8:17   0    2G  0 part
      │ └─md0                     9:0    0    2G  0 raid1
      └─sdb2                      8:18   0  511M  0 part
      sdc                         8:32   0  2.5G  0 disk
      ├─sdc1                      8:33   0    2G  0 part
      │ └─md0                     9:0    0    2G  0 raid1
      └─sdc2                      8:34   0  511M  0 part
      ```

7) Соберите mdadm RAID0 на второй паре маленьких разделов.

      ```
      vagrant@vagrant:~$ sudo mdadm --create /dev/md1 --level=0 --raid-devices=2 /dev/sd[bc]2
      mdadm: Defaulting to version 1.2 metadata
      mdadm: array /dev/md1 started.
      vagrant@vagrant:~$ lsblk
      NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
      loop0                       7:0    0 55.4M  1 loop  /snap/core18/2128
      loop1                       7:1    0 61.9M  1 loop  /snap/core20/1328
      loop2                       7:2    0 61.9M  1 loop  /snap/core20/1361
      loop3                       7:3    0 55.5M  1 loop  /snap/core18/2284
      loop4                       7:4    0 67.2M  1 loop  /snap/lxd/21835
      loop5                       7:5    0 43.6M  1 loop  /snap/snapd/14978
      loop6                       7:6    0 67.9M  1 loop  /snap/lxd/22526
      sda                         8:0    0   64G  0 disk
      ├─sda1                      8:1    0    1M  0 part
      ├─sda2                      8:2    0    1G  0 part  /boot
      └─sda3                      8:3    0   63G  0 part
        └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm   /
      sdb                         8:16   0  2.5G  0 disk
      ├─sdb1                      8:17   0    2G  0 part
      │ └─md0                     9:0    0    2G  0 raid1
      └─sdb2                      8:18   0  511M  0 part
        └─md1                     9:1    0 1018M  0 raid0
      sdc                         8:32   0  2.5G  0 disk
      ├─sdc1                      8:33   0    2G  0 part
      │ └─md0                     9:0    0    2G  0 raid1
      └─sdc2                      8:34   0  511M  0 part
        └─md1                     9:1    0 1018M  0 raid0
      ```
      
8) Создайте 2 независимых PV на получившихся md-устройствах.

  ```
  root@vagrant:/home/vagrant# pvcreate /dev/md0
    Physical volume "/dev/md0" successfully created.
  root@vagrant:/home/vagrant# pvcreate /dev/md1
    Physical volume "/dev/md1" successfully created.
  root@vagrant:/home/vagrant# pvdisplay
    --- Physical volume ---
    PV Name               /dev/sda3
    VG Name               ubuntu-vg
    PV Size               <63.00 GiB / not usable 0
    Allocatable           yes
    PE Size               4.00 MiB
    Total PE              16127
    Free PE               8063
    Allocated PE          8064
    PV UUID               sDUvKe-EtCc-gKuY-ZXTD-1B1d-eh9Q-XldxLf

    "/dev/md0" is a new physical volume of "<2.00 GiB"
    --- NEW Physical volume ---
    PV Name               /dev/md0
    VG Name
    PV Size               <2.00 GiB
    Allocatable           NO
    PE Size               0
    Total PE              0
    Free PE               0
    Allocated PE          0
    PV UUID               EKXuM5-7g5K-Q1mq-4Mex-dNTt-xwV1-6PodRs

    "/dev/md1" is a new physical volume of "1018.00 MiB"
    --- NEW Physical volume ---
    PV Name               /dev/md1
    VG Name
    PV Size               1018.00 MiB
    Allocatable           NO
    PE Size               0
    Total PE              0
    Free PE               0
    Allocated PE          0
    PV UUID               5Whf8K-3cfs-xcLj-oy1v-0txo-RFUu-HEq5qJ
  ```

9) Создайте общую volume-group на этих двух PV.

  ```
  root@vagrant:/home/vagrant# vgcreate VG_mine /dev/md0 /dev/md1
    Volume group "VG_mine" successfully created
  root@vagrant:/home/vagrant# pvs
    PV         VG        Fmt  Attr PSize    PFree
    /dev/md0   VG_mine   lvm2 a--    <2.00g   <2.00g
    /dev/md1   VG_mine   lvm2 a--  1016.00m 1016.00m
    /dev/sda3  ubuntu-vg lvm2 a--   <63.00g  <31.50g
  ```

10) Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.

  ```
  root@vagrant:/home/vagrant# lvcreate -L 100M VG_mine /dev/md1
    Logical volume "lvol0" created.
  root@vagrant:/home/vagrant# lsblk
  NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
  loop0                       7:0    0 55.4M  1 loop  /snap/core18/2128
  loop1                       7:1    0 61.9M  1 loop  /snap/core20/1328
  loop2                       7:2    0 61.9M  1 loop  /snap/core20/1361
  loop3                       7:3    0 55.5M  1 loop  /snap/core18/2284
  loop4                       7:4    0 67.2M  1 loop  /snap/lxd/21835
  loop5                       7:5    0 43.6M  1 loop  /snap/snapd/14978
  loop6                       7:6    0 67.9M  1 loop  /snap/lxd/22526
  sda                         8:0    0   64G  0 disk
  ├─sda1                      8:1    0    1M  0 part
  ├─sda2                      8:2    0    1G  0 part  /boot
  └─sda3                      8:3    0   63G  0 part
    └─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm   /
  sdb                         8:16   0  2.5G  0 disk
  ├─sdb1                      8:17   0    2G  0 part
  │ └─md0                     9:0    0    2G  0 raid1
  └─sdb2                      8:18   0  511M  0 part
    └─md1                     9:1    0 1018M  0 raid0
      └─VG_mine-lvol0       253:1    0  100M  0 lvm
  sdc                         8:32   0  2.5G  0 disk
  ├─sdc1                      8:33   0    2G  0 part
  │ └─md0                     9:0    0    2G  0 raid1
  └─sdc2                      8:34   0  511M  0 part
    └─md1                     9:1    0 1018M  0 raid0
      └─VG_mine-lvol0       253:1    0  100M  0 lvm
  ```

11) Создайте mkfs.ext4 ФС на получившемся LV.

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
