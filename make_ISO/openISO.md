# loongsonISO制作

## 准备阶段
  - 在龙芯官网下载最新版本的服务端的操作系统镜像。

## 修改initrd及相关标志性文件
 1. 将下载的镜像挂载到系统的mnt目录下
 2. 将/mnt目录下的全部文件拷贝到工作目录下
 3. 解压initrd文件
 ```
 cp initrd initrd.gz
 gunzip -d initrd.gz
 cpio -idmv < inird
 ```

4. 修改/boot文件下的boot.cfg和grub.cfg，将显示的字符修改为SIAS
5. 替换license文件，将license文件替换成redfalg的license（在/usr/share/doc/redhat-release-xxx文件夹下）
5. 将修改好的文件夹重新打包成tar.gz格式的压缩包


## 重新打包iso
1. 创建临时目录tmp
2. 在tmp目录下创建boot和live文件夹
3. 将修改好的initrd文件拷贝到boot文件夹下，将修改好的tar.gz文件拷贝到live文件夹下
4. 将/mnt/boot/文件夹下的grub.efi、vmlinux、vmlinux-xx文件拷贝到boot文件夹下
5. 将tmp文件夹下的文件制作成iso
```
mkisofs -relaxed-filenames -allow-lowercase -graft-points -allow-multidot -pad -r -l -J -d -v -V "SIAS" -hide-rr-moved -o SIAS-`date "+%y%m%d-%H%M%S"`.iso ISO/
```


## ISO光盘机U盘安装方法
- PMON固件
   - 光盘安装
      需要选则Sata Cdrom选项进行安装
   - U盘安装
      需要选择Usb Cdrom选项进行安装，在选择此选项之后需要等待5分钟
- 昆仑固件
   - 光盘安装
     1. 将机器的启动方式修改成mksh方式。
     2. 输入命令`connect`
     3. 输入命令`device`查看连接的设备
     4. 输入命令`cd fs0:`进入光盘
     5. 输入命令`cd boot`进入boot目录
     6. 输入命令`initrd initrd`加载initrd文件
     7. 输入命令`run vmlinux console=tty`启动光盘中的系统
     8. 系统安装
   - U盘安装
     1. 将机器的启动方式修改成mksh的方式
     2. 输入命令`connect`
     3. 输入命令`device`查看连接的设备
     4. 输入命令`cd fs1:`进入U盘
     5. 输入命令`cd boot`进入boot目录
     6. 输入命令`initrd initrd`加载initrd文件
     7. 输入命令`run vmlinux console=tty`启动U盘中的系统
     8. 系统安装
