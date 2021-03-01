### Linux-挂载存储设备

1.vfat文件编码方式
fat32文件名分为两种,短文件名和长文件名
两种文件名在磁盘上的存储方式是不同的,长文件名在目录项中特殊的标记
短文件名也就是8.3格式,对于包含中文的任何文件来说都不可能是短文件名
root@ubuntu:/home/zthong/Desktop/android-sdk-linux# fdisk -l

Disk /dev/sda: 34.4 GB, 34359738368 bytes
255 heads, 63 sectors/track, 4177 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0005b8c9

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *           1        4000    32125952   83  Linux
/dev/sda2            4000        4178     1425409    5  Extended
/dev/sda5            4000        4178     1425408   82  Linux swap / Solaris

Disk /dev/sdb: 3963 MB, 3963616768 bytes
255 heads, 63 sectors/track, 481 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0000231f

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1              17         481     3735112+   b  W95 FAT32
root@ubuntu:/home/zthong/Desktop/android-sdk-linux# mkdir /mnt/usb
root@ubuntu:/home/zthong/Desktop/android-sdk-linux# ls /mnt/usb/
root@ubuntu:/home/zthong/Desktop/android-sdk-linux# mount -t vfat /dev/sdb
sdb   sdb1 
root@ubuntu:/home/zthong/Desktop/android-sdk-linux# mount -t vfat /dev/sdb1 /mnt/usb/
