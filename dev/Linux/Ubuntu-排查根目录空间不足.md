### 排查ubuntu根目录空间不足



```
root@ubuntu:/# df -h
文件系统        容量  已用  可用 已用% 挂载点
udev            1.9G     0  1.9G    0% /dev
tmpfs           378M   12M  367M    4% /run
/dev/sda1        29G   27G  314M   99% /
tmpfs           1.9G  1.1M  1.9G    1% /dev/shm
tmpfs           5.0M  4.0K  5.0M    1% /run/lock
tmpfs           1.9G     0  1.9G    0% /sys/fs/cgroup
/dev/sda6       232M   59M  158M   27% /boot
/dev/sda7       880G  694G  142G   84% /home
```



```
root@ubuntu:/# du -h --max-depth=1
9.5M    ./root
57M     ./boot
188K    ./dev
13M     ./bin
4.0K    ./srv
0       ./proc
8.0K    ./media
5.3M    ./tmp
23G     ./var
13M     ./sbin
3.9M    ./lib32
4.0K    ./lib64
261M    ./opt
0       ./sys
4.0K    ./cdrom
4.6M    ./libx32
```



var目录下占用大，

```
root@ubuntu:/var# du -h --max-depth=1
1.4M    ./spool
584K    ./log
4.0K    ./metrics
68K     ./tmp
124M    ./cache
4.0K    ./local
4.0K    ./opt
236K    ./mail
4.0K    ./crash
23G     ./lib
5.4M    ./backups
23G     .
```



继续

```
root@ubuntu-Shangqi-N720:/var/lib# du -h --max-depth=1
4.0K    ./git
8.0K    ./xkb
4.0K    ./update-rc.d
4.0K    ./os-prober
8.0K    ./sudo
4.0K    ./ntpdate
4.0K    ./upower
8.0K    ./update-manager
52K     ./polkit-1
4.0K    ./deborphan
4.0K    ./acpi-support
48K     ./nssdb
28K     ./pam
8.0K    ./yum
4.0K    ./udisks2
4.0K    ./ubuntu-release-upgrader
8.0K    ./ubuntu-drivers-common
8.0K    ./logrotate
3.6M    ./aspell
4.0K    ./initscripts
8.0K    ./xfonts
4.0K    ./snmp
1.4M    ./influxdb
20K     ./update-notifier
8.0K    ./whoopsie
12K     ./sgml-base
68M     ./dpkg
20K     ./gconf
544K    ./usbutils
356K    ./systemd
16K     ./locales
4.0K    ./rpm
4.0K    ./urandom
8.0K    ./postfix
36K     ./ghostscript
24K     ./lightdm-data
4.0K    ./dbus
208M    ./apt
32K     ./NetworkManager
12K     ./apparmor
24K     ./emacsen-common
2.2M    ./samba
4.0K    ./ubiquity
32K     ./AccountsService
184K    ./doc-base
4.0K    ./avahi-autoipd
4.0K    ./dhcp
12K     ./xml-core
8.0K    ./ispell
4.0K    ./insserv
4.0K    ./resolvconf
4.0K    ./misc
4.0K    ./bluetooth
536M    ./mlocate
656K    ./lightdm
16K     ./alsa
16K     ./libreoffice
4.0K    ./usb_modeswitch
24K     ./dictionaries-common
4.0K    ./hp
24K     ./colord
13M     ./app-info
8.0K    ./ureadahead
8.0K    ./fwupd
128K    ./ucf
8.0K    ./initramfs-tools
4.0K    ./mongodb
8.0K    ./vim
22G     ./docker
8.0K    ./plymouth
4.0K    ./python
4.0K    ./man-db
23G     .
root@ubuntu-Shangqi-N720:/var/lib#
```



docker目录大

```
root@ubuntu:/var/lib# docker info | grep 'Docker Root Dir'
WARNING: No swap limit support
Docker Root Dir: /var/lib/docker
```



```
root@ubuntu:/var/lib# docker system df
TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              26                  9                   5.499GB             3.071GB (55%)
Containers          9                   7                   6.489MB             6.456MB (99%)
Local Volumes       68                  19                  8.967GB             4.152GB (46%)
```



image占用大，所以清理一下无用的。