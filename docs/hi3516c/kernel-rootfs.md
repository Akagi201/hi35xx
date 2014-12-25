## compile kernel

* `cd linux-3.0.y`
* `./mk3518.sh`

`mk3518.sh`脚本里面包含了编译linux内核所需的所有的命令, 具体看`mk3518.sh`文件, 编译成功将生成在目录`arch/arm/boot/uImage`文件, 这个就是要烧录到板子上的.

## 制作`rootfs`文件系统
* `./mkfs.jffs2 -d ./rootfs -l -e 0x10000 -o rootfs.jffs2`

生成的`rootfs.jffs2`这个就是要烧录到板子上的文件.

## flash地址空间说明

|     1M     |      3M       |      12M              |        |------------|---------------|-----------------------|
|    boot    |     kernel    |     rootfs            |

## `uboot`烧写内核
* `set serverip 192.168.1.201` // tftpserver主机ip
* `mw.b 0x82000000 0xFF 0x300000`
* `tftp 0x82000000 uImage`
* `sf probe 0`
* `sf erase 0x100000 0x300000`
* `sf write 0x82000000 0x100000 0x300000`

## 烧写`rootfs`文件系统

* `mw.b 0x82000000 0xFF 0xc00000`
* `tftp 0x82000000 rootfs.jffs2`
* `sf probe 0`
* `sf erase 0x400000 0xc00000`
* `sf write 0x82000000 0x400000 0xc00000`

## 设置启动参数和启动命令

* `setenv bootargs 'mem=64M console=ttyAMA0,115200 root=/dev/mtdblock2 rootfstype=jffs2 mtdparts=hi_sfc:1M(boot),3M(kernel),12M(rootfs)'`
* `setenv bootcmd 'sf probe 0;sf read 0x82000000 0x100000 0x300000;bootm 0x82000000'`
* `saveenv`
* `reset`
