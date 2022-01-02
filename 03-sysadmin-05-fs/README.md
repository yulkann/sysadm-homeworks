# Домашнее задание к занятию "3.5. Файловые системы"

1. Узнайте о [sparse](https://ru.wikipedia.org/wiki/%D0%A0%D0%B0%D0%B7%D1%80%D0%B5%D0%B6%D1%91%D0%BD%D0%BD%D1%8B%D0%B9_%D1%84%D0%B0%D0%B9%D0%BB) (разряженных) файлах.

        разреженные файлы являются специальным форматом представления, в котором часть цифровой последовательности заменена сведениями о ней (сформирован перечень дыр), что в свою очередь позволяет гораздо эффективнее задействовать возможности файловой системы. 

1. Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?

         не могут, у хард линков те же права и владелец что и у основного файла 

1. Сделайте `vagrant destroy` на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:

    ```bash
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
        PS D:\DevOps\VM> cat Vagrantfile
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

    Данная конфигурация создаст новую виртуальную машину с двумя дополнительными неразмеченными дисками по 2.5 Гб.

1. Используя `fdisk`, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.

         vagrant@vagrant:~$ sudo fdisk /dev/sdb

                    Welcome to fdisk (util-linux 2.34).
                    Changes will remain in memory only, until you decide to write them.
                    Be careful before using the write command.

                    Device does not contain a recognized partition table.
                    Created a new DOS disklabel with disk identifier 0x230de6e3.

                    Command (m for help): p
                    Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors
                    Disk model: VBOX HARDDISK
                    Units: sectors of 1 * 512 = 512 bytes
                    Sector size (logical/physical): 512 bytes / 512 bytes
                    I/O size (minimum/optimal): 512 bytes / 512 bytes
                    Disklabel type: dos
                    Disk identifier: 0x230de6e3

                    Command (m for help): n
                    Partition type
                       p   primary (0 primary, 0 extended, 4 free)
                       e   extended (container for logical partitions)
                    Select (default p): p
                    Partition number (1-4, default 1): 1
                    First sector (2048-5242879, default 2048): 2048
                    Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879): +2048M

                    Created a new partition 1 of type 'Linux' and of size 2 GiB.

                    Command (m for help): p
                    Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors
                    Disk model: VBOX HARDDISK
                    Units: sectors of 1 * 512 = 512 bytes
                    Sector size (logical/physical): 512 bytes / 512 bytes
                    I/O size (minimum/optimal): 512 bytes / 512 bytes
                    Disklabel type: dos
                    Disk identifier: 0x230de6e3

                    Device     Boot Start     End Sectors Size Id Type
                    /dev/sdb1        2048 4196351 4194304   2G 83 Linux

                    Command (m for help): n
                    Partition type
                       p   primary (1 primary, 0 extended, 3 free)
                       e   extended (container for logical partitions)
                    Select (default p): p
                    Partition number (2-4, default 2): 2
                    First sector (4196352-5242879, default 4196352): 4196352
                    Last sector, +/-sectors or +/-size{K,M,G,T,P} (4196352-5242879, default 5242879): 5242879

                    Created a new partition 2 of type 'Linux' and of size 511 MiB.

                    Command (m for help): p
                    Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors
                    Disk model: VBOX HARDDISK
                    Units: sectors of 1 * 512 = 512 bytes
                    Sector size (logical/physical): 512 bytes / 512 bytes
                    I/O size (minimum/optimal): 512 bytes / 512 bytes
                    Disklabel type: dos
                    Disk identifier: 0x230de6e3

                    Device     Boot   Start     End Sectors  Size Id Type
                    /dev/sdb1          2048 4196351 4194304    2G 83 Linux
                    /dev/sdb2       4196352 5242879 1046528  511M 83 Linux

                    Command (m for help): w
                    The partition table has been altered.
                    Calling ioctl() to re-read partition table.
                    Syncing disks.

1. Используя `sfdisk`, перенесите данную таблицу разделов на второй диск.

                    vagrant@vagrant:~$ sudo sfdisk -d /dev/sdb | sudo sfdisk /dev/sdc
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
                    >>> Created a new DOS disklabel with disk identifier 0x230de6e3.
                    /dev/sdc1: Created a new partition 1 of type 'Linux' and of size 2 GiB.
                    /dev/sdc2: Created a new partition 2 of type 'Linux' and of size 511 MiB.
                    /dev/sdc3: Done.

                    New situation:
                    Disklabel type: dos
                    Disk identifier: 0x230de6e3

                    Device     Boot   Start     End Sectors  Size Id Type
                    /dev/sdc1          2048 4196351 4194304    2G 83 Linux
                    /dev/sdc2       4196352 5242879 1046528  511M 83 Linux

                    The partition table has been altered.
                    Calling ioctl() to re-read partition table.
                    Syncing disks.
                    vagrant@vagrant:~$ sudo fdisk -l
                    Disk /dev/sda: 64 GiB, 68719476736 bytes, 134217728 sectors
                    Disk model: VBOX HARDDISK
                    Units: sectors of 1 * 512 = 512 bytes
                    Sector size (logical/physical): 512 bytes / 512 bytes
                    I/O size (minimum/optimal): 512 bytes / 512 bytes
                    Disklabel type: dos
                    Disk identifier: 0x3f94c461

                    Device     Boot   Start       End   Sectors  Size Id Type
                    /dev/sda1  *       2048   1050623   1048576  512M  b W95 FAT32
                    /dev/sda2       1052670 134215679 133163010 63.5G  5 Extended
                    /dev/sda5       1052672 134215679 133163008 63.5G 8e Linux LVM


                    Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors
                    Disk model: VBOX HARDDISK
                    Units: sectors of 1 * 512 = 512 bytes
                    Sector size (logical/physical): 512 bytes / 512 bytes
                    I/O size (minimum/optimal): 512 bytes / 512 bytes
                    Disklabel type: dos
                    Disk identifier: 0x230de6e3

                    Device     Boot   Start     End Sectors  Size Id Type
                    /dev/sdb1          2048 4196351 4194304    2G 83 Linux
                    /dev/sdb2       4196352 5242879 1046528  511M 83 Linux


                    Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
                    Disk model: VBOX HARDDISK
                    Units: sectors of 1 * 512 = 512 bytes
                    Sector size (logical/physical): 512 bytes / 512 bytes
                    I/O size (minimum/optimal): 512 bytes / 512 bytes
                    Disklabel type: dos
                    Disk identifier: 0x230de6e3

                    Device     Boot   Start     End Sectors  Size Id Type
                    /dev/sdc1          2048 4196351 4194304    2G 83 Linux
                    /dev/sdc2       4196352 5242879 1046528  511M 83 Linux


                    Disk /dev/mapper/vgvagrant-root: 62.55 GiB, 67150807040 bytes, 131153920 sectors
                    Units: sectors of 1 * 512 = 512 bytes
                    Sector size (logical/physical): 512 bytes / 512 bytes
                    I/O size (minimum/optimal): 512 bytes / 512 bytes


                    Disk /dev/mapper/vgvagrant-swap_1: 980 MiB, 1027604480 bytes, 2007040 sectors
                    Units: sectors of 1 * 512 = 512 bytes
                    Sector size (logical/physical): 512 bytes / 512 bytes
                    I/O size (minimum/optimal): 512 bytes / 512 bytes

1. Соберите `mdadm` RAID1 на паре разделов 2 Гб.

                    vagrant@vagrant:~$ sudo mdadm --create --verbose /dev/md1 --level=1 --raid-device=2 /dev/sdb1 /dev/sdc1
                    mdadm: Note: this array has metadata at the start and
                        may not be suitable as a boot device.  If you plan to
                        store '/boot' on this device please ensure that
                        your boot-loader understands md/v1.x metadata, or use
                        --metadata=0.90
                    mdadm: size set to 2094080K
                    Continue creating array? y
                    mdadm: Defaulting to version 1.2 metadata
                    mdadm: array /dev/md1 started.

1. Соберите `mdadm` RAID0 на второй паре маленьких разделов.

                    vagrant@vagrant:~$ sudo mdadm --create --verbose /dev/md0 --level=1 --raid-device=2 /dev/sdb2 /dev/sdc2
                    mdadm: Note: this array has metadata at the start and
                        may not be suitable as a boot device.  If you plan to
                        store '/boot' on this device please ensure that
                        your boot-loader understands md/v1.x metadata, or use
                        --metadata=0.90
                    mdadm: size set to 522240K
                    Continue creating array? y
                    mdadm: Defaulting to version 1.2 metadata
                    mdadm: array /dev/md0 started.

1. Создайте 2 независимых PV на получившихся md-устройствах.

                    vagrant@vagrant:~$ sudo pvcreate /dev/md1 /dev/md0
                      Physical volume "/dev/md1" successfully created.
                      Physical volume "/dev/md0" successfully created.

1. Создайте общую volume-group на этих двух PV.

                    vagrant@vagrant:~$ sudo vgcreate vg1 /dev/md1 /dev/md0
                      Volume group "vg1" successfully created

1. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.

                    vagrant@vagrant:~$ sudo lvcreate -L 100M vg1 /dev/md0
                      Logical volume "lvol0" created.

1. Создайте `mkfs.ext4` ФС на получившемся LV.

                    vagrant@vagrant:~$ sudo mkfs.ext4 /dev/vg1/lvol0
                    mke2fs 1.45.5 (07-Jan-2020)
                    Creating filesystem with 25600 4k blocks and 25600 inodes

                    Allocating group tables: done
                    Writing inode tables: done
                    Creating journal (1024 blocks): done
                    Writing superblocks and filesystem accounting information: done

1. Смонтируйте этот раздел в любую директорию, например, `/tmp/new`.

                    vagrant@vagrant:~$ mkdir /tmp/new
                    vagrant@vagrant:~$ mount /dev/vg1/lvol0 /tmp/new
                    mount: only root can do that
                    vagrant@vagrant:~$ sudo mount /dev/vg1/lvol0 /tmp/new

1. Поместите туда тестовый файл, например `wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz`.


                    vagrant@vagrant:~$ sudo wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/testfile.gz
                    --2022-01-02 15:46:47--  https://mirror.yandex.ru/ubuntu/ls-lR.gz
                    Resolving mirror.yandex.ru (mirror.yandex.ru)... 213.180.204.183, 2a02:6b8::183
                    Connecting to mirror.yandex.ru (mirror.yandex.ru)|213.180.204.183|:443... connected.
                    HTTP request sent, awaiting response... 200 OK
                    Length: 21507191 (21M) [application/octet-stream]
                    Saving to: ‘/tmp/new/testfile.gz’

                    /tmp/new/testfile.gz       100%[======================================>]  20.51M  4.44MB/s    in 7.2s

                    2022-01-02 15:46:54 (2.84 MB/s) - ‘/tmp/new/testfile.gz’ saved [21507191/21507191]

                    vagrant@vagrant:~$ ls /tmp/new/
                    lost+found  testfile.gz

1. Прикрепите вывод `lsblk`.

                        vagrant@vagrant:~$ lsblk
                        NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
                        sda                    8:0    0   64G  0 disk
                        ├─sda1                 8:1    0  512M  0 part  /boot/efi
                        ├─sda2                 8:2    0    1K  0 part
                        └─sda5                 8:5    0 63.5G  0 part
                          ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
                          └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
                        sdb                    8:16   0  2.5G  0 disk
                        ├─sdb1                 8:17   0    2G  0 part
                        │ └─md1                9:1    0    2G  0 raid1
                        └─sdb2                 8:18   0  511M  0 part
                          └─md0                9:0    0  510M  0 raid1
                            └─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/new
                        sdc                    8:32   0  2.5G  0 disk
                        ├─sdc1                 8:33   0    2G  0 part
                        │ └─md1                9:1    0    2G  0 raid1
                        └─sdc2                 8:34   0  511M  0 part
                          └─md0                9:0    0  510M  0 raid1
                            └─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/new

1. Протестируйте целостность файла:

    ```bash
    root@vagrant:~# gzip -t /tmp/new/test.gz
    root@vagrant:~# echo $?
    0
    ```
                        vagrant@vagrant:~$ sudo gzip -t /tmp/new/testfile.gz
                        vagrant@vagrant:~$ echo $?
                        0

1. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.

                        vagrant@vagrant:~$ sudo pvmove /dev/md0
                          /dev/md0: Moved: 24.00%
                          /dev/md0: Moved: 100.00%

1. Сделайте `--fail` на устройство в вашем RAID1 md.

                        vagrant@vagrant:~$ sudo mdadm /dev/md1 --fail /dev/sdb1
                        mdadm: set /dev/sdb1 faulty in /dev/md1
                        
                        vagrant@vagrant:~$ cat /proc/mdstat
                        Personalities : [linear] [multipath] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10]
                        md1 : active raid1 sdc1[1] sdb1[0](F)
                              2094080 blocks super 1.2 [2/1] [_U]

                        md0 : active raid1 sdc2[1] sdb2[0]
                              522240 blocks super 1.2 [2/2] [UU]

                        unused devices: <none>



1. Подтвердите выводом `dmesg`, что RAID1 работает в деградированном состоянии.


                        vagrant@vagrant:~$ dmesg |grep md1
                        [  348.952483] md/raid1:md1: not clean -- starting background reconstruction
                        [  348.952485] md/raid1:md1: active with 2 out of 2 mirrors
                        [  348.952516] md1: detected capacity change from 0 to 2144337920
                        [  348.958462] md: resync of RAID array md1
                        [  359.511682] md: md1: resync done.
                        [ 1165.206285] md/raid1:md1: Disk failure on sdb1, disabling device.
                                       md/raid1:md1: Operation continuing on 1 devices.

1. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:

    ```bash
    root@vagrant:~# gzip -t /tmp/new/test.gz
    root@vagrant:~# echo $?
    0
    ```
                                vagrant@vagrant:~$ sudo gzip -t /tmp/new/testfile.gz
                                vagrant@vagrant:~$ echo $?
                                0
                                
1. Погасите тестовый хост, `vagrant destroy`.

                            PS D:\DevOps\VM> vagrant destroy
                                default: Are you sure you want to destroy the 'default' VM? [y/N] y
                            ==> default: Forcing shutdown of VM...
                            ==> default: Destroying VM and associated drives...
