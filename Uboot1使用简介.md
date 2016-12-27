# Uboot1使用简介

---

## 在uboot1中启动系统

- **printenv**，显示当前的uboot环境变量

- **setenv serverip 192.168.1.136; setenv ipaddr 192.168.1.123; setenv gatewayip 192.168.1.1**

  > - setenv, 保存uboot环境变量
  > - serverip, 服务器地址；ipaddr，本机地址；gatewayip，网关地址；
      这样设置好后，就可以使用tftp从服务器下载远程文件（比如uImage）

- **ping 192.168.1.136**，检查网络状况

- **setenv bootargs console=ttyAMA3,115200 vmalloc=304M mem=255M rw ip=192.168.1.123 root=/dev/nfs nfsroot=192.168.1.136:/home/tim/system_mine,nolock serialno=iMAPb1c2a1a3**

  > 设置传递给kernel的启动参数，此启动参数可以挂载NFS根文件系统

- **setenv bootargs console=ttyAMA3,115200 vmalloc=304M mem=255M rootfstype=squashfs root=/dev/spiblock1 rw serialno=iMAPb1c2a1a3**

  > 设置传递给kernel的启动参数，此启动参数可以启动Flash上的根文件系统

- **saveenv**, 保存uboot环境变量；可以不做这一步，这样重启机器后还是原来的环境变量设定

- **tftp 80008000 uImage**

  > 将远程文件uImage下载到内存地址为0x80008000的地方

- **bootm 80008000**

  > 启动放在内存地址0x80008000的uImage，接着就能进入下根文件系统

## 在uboot1中对Flash擦除和烧录镜像

- **vs**, 对Flash做写入和擦除，直接敲入可以显示相关子命令

  > vs info
  > vs assign number
  > vs reset
  > vs read (part) (memory address) (device start address) len
  > vs write (part) (memory address) (device start address) len
  > vs erase (part) (device start address) len
  > vs scrub (part) (memory address) (device start address) len

- **vs info**, 显示当前Flash的分区结构

  > Part list:
  > name:uboot offset:0 size:c000
  > name:item offset:c000 size:4000
  > name:kernel0 offset:20000 size:1f0000
  > name:system offset:290000 size:7d0000
  > name:config offset:a60000 size:fffffffffffffc00
  > name:uboot1 offset:210000 size:80000
  > name:reserved offset:10400 size:fc00
  >
  > 其中uboot和item固定占据Flash头64KB大小的位置

- **vs write**, 写入分区

  > 从（memory address）处读取内容，写入到Flash地址(device start address)，大小为len

- **vs erase**, 擦除分区，最小单位是64KB大小

  > 故uboot和item每次都是一并擦除掉的，擦除后两个分区都需要分别重新写入

- **使用过程**

  > - 先用tftp下载到一个内存地址上（比如0x80008000）
  > - 使用vs erase擦除对应的分区
  > - 使用vs write写入分区
  
[back](./)
