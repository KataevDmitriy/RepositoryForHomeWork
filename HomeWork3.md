
**подключил 3 диска в VM**

**просмотрел блочные устройства**
    lsblk 

**установил LVM**
    sudo apt install lvm2

**Создал разделы на подключенных дисках** 
    fdisk /dev/vdb
    fdisk /dev/vdc
    fdisk /dev/vdd


**инициализировал физические LVM разделы** 
*(использовал ресурс https://losst.ru/sozdanie-i-nastrojka-lvm-linux)*
    sudo pvcreate /dev/vdb1 /dev/vdc1 /dev/vdd1
**просмотрел физические LVM разделы**
    sudo pvscan
    sudo pvdisplay


**создал группу разделов LVM**
    sudo vgcreate vol_grp1 /dev/vdb1 /dev/vdc1 /dev/vdd1
**просмотрел группу разделов**
    sudo vgdisplay


**создал логические разделы LVM размером 1G**
    sudo lvcreate -L1G -n logical_vol1 vol_grp1
    sudo lvcreate -L1G -n logical_vol2 vol_grp1
    sudo lvcreate -L1G -n logical_vol3 vol_grp1

**создал файловую систему ext4 на первом логическом разделе logical_vol1**
    sudo mkfs.ext4 /dev/vol_grp1/logical_vol1
**примонтировал логический раздел к директории /var/log**
    sudo mount /dev/vol_grp1/logical_vol1 /var/log
**посмотрел файловую систему на точке монтирования /var/log**
    df -h /var/log


**установил пакет xfsprogs для поддержки mkfs.xfs**
    sudo apt-get install xfsprogs

**создал файловую систему xfs на втором логическом разделе logical_vol2**
    sudo mkfs -t xfs /dev/vol_grp1/logical_vol2
**примонтировал логический раздел к директории /var/lib/db**
    sudo mount /dev/vol_grp1/logical_vol2 /var/lib/db
**посмотрел файловую систему на точке монтирования /var/lib/db**
    df -h /var/lib/db:


**создал swap раздел**
*(использовал ресурс https://askubuntu.com/questions/33697/how-do-i-add-swap-after-system-installation)*
    sudo mkswap /dev/vol_grp1/logical_vol3
**включил swap раздел**
    sudo swapon -U 79324b14-dcd8-4250-ad59-ea3dab3b7227
**просмотрел свап раздел в системе**
    sudo swapon —show

**просмотрел UUID разделов для добавления в /etc/fstab**
    blkid /dev/vol_grp1/logical_vol1
    blkid /dev/vol_grp1/logical_vol2
    blkid /dev/vol_grp1/logical_vol3

**Добавил строки в /etc/fstab для автоматического монтирования логических дисков к точкам монтирования**
    UUID=dd20bc94-08b8-462f-8a31-927e73cff2d0 /var/log ext4 default 0 0
    UUID=549e99e7-bec7-4afb-b1fb-7edb91a2e2ed /var/lib/db xfs default 0 0
    UUID=79324b14-dcd8-4250-ad59-ea3dab3b7227 none swap sw 0 0