## compile
* `cd u-boot-2010.06`
* `./mk3518.sh`

`mk3518.sh`脚本里面包含了编译`uboot`所需的所有的命令, 具体看`mk3518.sh`文件, 编译成功将生成`uboot-me.bin`文件, 这个就是要烧录到板子上的.

## 烧写u-boot
* 谨慎更新板子u-boot, 没有把握尽量不要重新烧写uboot, 烧写失败将无法启动, 切记.
* `mw.b 0x82000000 0xFF 0x40000`
* `set serverip 192.168.1.201` // tftpserver的ip
* `tftp 0x82000000 uboot-me.bin`
* `sf probe 0`
* `sf erase 0 0x40000` // 执行这步删除flash命令之前一定要tftp下载`uboot-me.bin`成功, 一定要小心确认, 烧写失败将无法启动, 切记.
* `sf write 0x82000000 0 0x40000`
* `reset`

## 烧写过程

```
烧录过程：
hisilicon # mw.b 0x82000000 0xFF 0x40000
hisilicon # set serverip 192.168.1.199
hisilicon # tftp 0x82000000 uboot-me.bin
Hisilicon ETH net controler
miiphy_register: non unique device name '0:1'
miiphy_register: non unique device name '0:2'
MAC:   00-00-23-34-45-66
UP_PORT : phy status change : LINK=DOWN : DUPLEX=FULL : SPEED=100M
UP_PORT : phy status change : LINK=UP : DUPLEX=FULL : SPEED=100M
TFTP from server 192.168.1.100; our IP address is 192.168.1.10
Download Filename 'uboot-me.bin'.
Download to address: 0x82000000
Downloading: #  [ Connected ]
####
done
Bytes transferred = 159204 (26de4 hex)
hisilicon # sf probe 0
16384 KiB hi_sfc at 0:0 is now current device
hisilicon # sf erase 0 0x40000
Erasing at 0x40000 -- 100% complete.
hisilicon # sf write 0x82000000 0 0x40000
Writing at 0x40000 -- 100% complete.
hisilicon # reset
```