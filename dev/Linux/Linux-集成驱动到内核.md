### Linux-集成驱动到内核

1). Copy the driver source  folder into drivers/net/wireless/ and rename it as
<folder_name>, rtl8192cu.

复制驱动代码到内核相应目录

2). Add the following line into drivers/net/wireless/Makefile, CONFIG_RTL8192CU
is for <compile_flag>, rtl8192cu is for <folder_name>:

在 drivers/net/wireless/Makefile文件中添加如下：
obj-$(CONFIG_RTL8192CU)  += rtl8192cu/ 

3). Add  the following line  into drivers/net/wireless/Kconfig, rtl8192cu is for
<folder_name>:

在drivers/net/wireless/Kconfig文件中添加如下：
source "drivers/net/wireless/rtl8192cu/Kconfig" 

4). Config kernel, for example, with ‘make menuconfig’ command to select ‘y’ or ‘m’
for our driver.

配置内核，例如：命令make menuconfig，选择‘y’或者‘m’

5). Now, you can build kernel with ‘make’ command.

编译内核