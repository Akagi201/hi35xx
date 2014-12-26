# Hi3518 SDK安装以及升级使用说明

## 第一章 Hi3518_SDK_Vx.x.x.x版本升级操作说明

如果您是首次安装本SDK, 请直接参看第2章.

## 第二章 首次安装SDK

1. Hi3518 SDK包位置

 在`Hi3518_V100R001SPC***/01.software/board`目录下, 您可以看到一个`Hi3518_SDK_Vx.x.x.x.tgz`的文件, 该文件就是Hi3518的软件开发包.

 其中, `Hi3518_V100R001SPC01xxx`对应的是`uclibc`版本, `Hi3518_V100R001SPC02xxx`对应的是`glibc`版本.

2. 解压缩SDK包

 在linux服务器上(或者一台装有linux的PC上, 主流的linux发行版本均可以), 使用命令: `tar -zxf Hi3518_SDK_Vx.x.x.x.tgz`解压缩该文件, 可以得到一个`Hi3518_SDK_Vx.x.x.x`目录.

3. 展开SDK包内容

 1) 在执行安装脚本前建议修改系统默认shell为`bash`.
 
 2) 返回`Hi3518_SDK_Vx.x.x.x`目录, 运行`source sdk.unpack`(请用root或sudo权限执行)将会展开SDK包打包压缩存放的内容, 请按照提示完成操作.

 如果您需要通过`WINDOWS`操作系统中转拷贝SDK包, 请先运行`source sdk.cleanup`, 收起SDK包的内容, 拷贝到新的目录后再展开.
 
4. 在linux服务器上安装交叉编译器

 1) 安装`uclibc`交叉编译器(注意, 需要有sudo权限或者root权限):
 
 进入`Hi3518_SDK_Vx.x.x.x/osdrv/toolchain/arm-hisiv100nptl-linux`目录, 运行`chmod +x cross.install`, 然后运行`./cross.install`即可.
 
 2) 安装glibc交叉编译器(注意, 需要有sudo权限或者root权限):
 
 进入`Hi3518_SDK_Vx.x.x.x/osdrv/toolchain/arm-hisiv200-linux`目录, 运行`chmod +x cross.install`, 然后运行`./cross.install`即可.
 
 3) 执行`source /etc/profile`, 安装交叉编译器的脚本配置的环境变量就可以生效了, 或者请重新登陆也可. `CROSS_COMPILER_PATH=/opt/hisi-linux/x86-arm/arm-hisiv200-linux/target/bin`
 
5. 编译osdrv

 参见`osdrv`目录下`readme`

6. SDK目录介绍

 `Hi3518_SDK_Vx.x.x.x`目录结构如下:
 
 ```
 |-- sdk.cleanup                 # SDK清理脚本
    |-- sdk.unpack                  # SDK展开脚本
    |-- osdrv                       # 存放操作系统及相关驱动的目录
    |   |-- busybox                 # busybox源代码
    |   |-- drv                     # drv源代码
    |   |-- kernel                  # linux内核源代码
    |   |-- pub                     # 编译好的镜像、工具、drv驱动等
    |   |-- rootfs_scripts          # rootfs源代码
    |   |-- toolchain               # 交叉编译器
    |   |-- tools                   # linux工具源代码
    |   |-- uboot                   # uboot源代码
    |   `-- Makefile                # osdrv Makefile
    |-- package                     # 存放SDK各种压缩包的目录
    |   |-- osdrv.tgz               # linux内核/uboot/rootfs/tools源码压缩包
    |   |-- mpp.tgz                 # 媒体处理平台软件压缩包
    |   `-- image                   # 可供FLASH烧写的映像文件，如内核、根文件系统
    |-- scripts                     # 存放shell脚本的目录
    |-- mpp                         # 存放媒体处理平台的目录
        |-- component               # 组件源代码 
        |-- extdrv                  # 板级外围驱动源代码
        |-- include                 # 对外头文件
        |-- ko                      # 内核模块
        |-- lib                     # release版本库以及音频库
        |-- tools                   # 媒体处理相关工具
        `-- sample                  # 样例源代码
 ```

## 第三章 安装, 升级Hi3518DEMO板开发开发环境

* 如果您使用的Hi3518的DEMO板, 可以按照以下步骤烧写u-boot, 内核以及文件系统, 以下步骤均使用网络来更新.
* 通常, 您拿到的单板中已经有烧写u-boot, 如果没有的话, 建议更换带u-boot的Flash.
* 更详细的操作步骤及说明, 请参见`01.software\board\documents`目录下的《Linux开发环境用户指南》.
* 以下操作假设您的单板上已经有`u-boot`, 使用网口烧写uboot, kernel及rootfs到Flash中.
* Demo单板默认为从`SPI Flash`启动.

1. 配置tftp服务器
 * 可以使用任意的tftp服务器.
 * 如果使用hi3518a, 将`package/image_uclibc_hi3518a`(或`image_glibc_hi3518a`)下的相关文件拷贝到tftp服务器目录下.
 * 如果使用hi3518c, 将`package/image_uclibc_hi3518c`(或`image_glibc_hi3518c`)下的相关文件拷贝到tftp服务器目录下.
 * 如果使用hi3516c, 则使用`package/image_uclibc_hi3516c`(或`image_glibc_hi3516c`)目录下的相关文件镜像.

2. 参数配置

 * 板上电后, 敲任意键进入u-boot. 设置`serverip`(即tftp服务器的ip), `ipaddr`(单板ip)和`ethaddr`(单板的MAC地址).
 
 ```
 setenv serverip xx.xx.xx.xx
 setenv ipaddr xx.xx.xx.xx 
 setenv ethaddr xx:xx:xx:xx:xx:xx
 setenv netmask xx.xx.xx.xx
 setenv gatewayip xx.xx.xx.xx
 ping serverip // 确保网络畅通
 ```
3. 烧写映像文件到SPI Flash

 以16M SPI Flash为例.

 1) 地址空间说明





