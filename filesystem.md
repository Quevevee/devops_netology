## 1. Узнайте о sparse (разряженных) файлах.  
Разрежённый файл — файл, в котором последовательности нулевых байтов заменены на информацию об этих последовательностях (список дыр).

## 2. Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?
Не могут, т.к. обе жесткие ссылки на один inode, где хранятся права доступа.

## 3. Сделайте vagrant destroy на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:

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


## 4. Используя fdisk, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.
    fdisk /dev/sdb    
    n
    Primary  
    +2G
    n
    Extended  
    <Enter>
    w
    
Результат:  

         Device: /dev/sdb1  
          Start: 2048  
            End: 4196351  
        Sectors: 4194304  
      Cylinders: 262  
           Size: 2G  
             Id: 83  
           Type: Linux  
    Start-C/H/S: 0/32/33  
      End-C/H/S: 261/53/48  
   

         Device: /dev/sdb2  
          Start: 4196352  
            End: 5242879  
        Sectors: 1046528  
      Cylinders: 66  
           Size: 511M  
             Id: 5  
           Type: Extended  
    Start-C/H/S: 261/53/49  
      End-C/H/S: 326/90/20  


## 5. Используя sfdisk, перенесите данную таблицу разделов на второй диск.
    sfdisk --dump /dev/sdb > ~/sdb.dump  
    sfdisk /dev/sdc < ~/sdb.dump  

## 6. Соберите mdadm RAID1 на паре разделов 2 Гб.
    mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1  

## 7. Соберите mdadm RAID0 на второй паре маленьких разделов.
    mdadm --create --verbose /dev/md1 --level=0 --raid-devices=2 /dev/sdb2 /dev/sdc2    
    
Результат:      
`cat /proc/mdstat`   
   `Personalities : [linear] [multipath] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10]`  
`md1 : active raid0 sdc2[1] sdb2[0]`  
      `1042432 blocks super 1.2 512k chunks`  
  
`md0 : active raid1 sdc1[1] sdb1[0]`  
      `2094080 blocks super 1.2 [2/2] [UU]`  
      
## 8. Создайте 2 независимых PV на получившихся md-устройствах.  

    pvcreate /dev/md0
      Physical volume "/dev/md0" successfully created.

    pvcreate /dev/md1
      Physical volume "/dev/md1" successfully created.

## 9. Создайте общую volume-group на этих двух PV.  
    vgcreate vgOne /dev/md0 /dev/md1

## 10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.
    lvcreate -L 100M -n lvOne vgOne /dev/md1

## 11. Создайте mkfs.ext4 ФС на получившемся LV.
    mkfs.ext4 /dev/vgOne/lvOne

## 12. Смонтируйте этот раздел в любую директорию, например, /tmp/new.
    mkdir /tmp/new && mount /dev/vgOne/lvOne /tmp/new

## 13. Поместите туда тестовый файл, например wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.
    wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz

## 14. Прикрепите вывод lsblk.
```bash
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
│ └─md0                9:0    0    2G  0 raid1  
└─sdb2                 8:18   0  511M  0 part  
  └─md1                9:1    0 1018M  0 raid0  
    └─vgOne-lvOne      253:2    0  100M  0 lvm   /tmp/new  
sdc                    8:32   0  2.5G  0 disk  
├─sdc1                 8:33   0    2G  0 part  
│ └─md0                9:0    0    2G  0 raid1  
└─sdc2                 8:34   0  511M  0 part  
  └─md1                9:1    0 1018M  0 raid0  
    └─vgOne-lvOne      253:2    0  100M  0 lvm   /tmp/new 
```
## 15. Протестируйте целостность файла:

    gzip -t /tmp/new/test.gz
    echo $?  
0

## 16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.
    pvmove /dev/md1 /dev/md0

## 17. Сделайте --fail на устройство в вашем RAID1 md.
    mdadm --fail /dev/md0

## 18. Подтвердите выводом dmesg, что RAID1 работает в деградированном состоянии.
    dmesg | grep raid
    ...  
    ...  
    [15364.224191] md/raid1:md0: not clean -- starting background reconstruction
    [15364.224192] md/raid1:md0: active with 2 out of 2 mirrors

## 19. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:

    gzip -t /tmp/new/test.gz
    echo $?
0  

## 20. Погасите тестовый хост, vagrant destroy.
Сделано.  После перемещения видно, что логический том находится на устройстве md0 (RAID1)

