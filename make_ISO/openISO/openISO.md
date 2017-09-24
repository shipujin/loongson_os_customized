# loongnix-Desktop-Fedora13-mipso32的iso制作

## 修改initrd及相关标志性文件
 1. 将下载的镜像挂载到系统的mnt目录下
 2. 将/mnt目录下的全部文件拷贝到工作目录下
 3. 解压initrd文件
 ```
 gunzip -d initrd.gz
 cpio -idmv < initrd
 ```
 4. 修改安装脚本，在script目录下的install脚本文件，将文件中的"Looongson"、“loongson”替换为SIAS。修改的效果是安装系统过程中桌面的左上角(Welcome to Loongson Linux System)、正中央方框内的内容(Installing loongson base system.)。
 5. 将修改好的initrd目录使用cpio工具重新做成备份档的包。
 ```
 find /initrd –type f | cpio –ocvB > ../initrd    //为了防止initrd目录名与备份档文件名同，报错
 ```

4. 由于在/boot文件下的boot.cfg和grub.cfg文件里没有原系统的字段“loongnix-Desktop-Fedora13-mipso32”，所以就不用替换为SIAS。
5. 替换license文件，将license文件替换成你要做的系统SIAS的license文件，在/usr/share/doc/下，但这个license不是loongson的license而是fedora的license文件。license是你遵循的开源协议。
6. 将修改好的initrd备份文件重新打包成原来的.gz格式的压缩包


## 重新打包iso
1. 创建临时目录tmp
2. 在tmp目录下创建boot和live文件夹
3. 将修改好的initrd文件拷贝到boot文件夹下，将修改好的.gz文件拷贝到live文件夹下
4. 将/mnt/boot/文件夹下的grub.efi、等文件拷贝到boot文件夹下
5. 将tmp文件夹下的文件制作成iso
```
mkisofs -relaxed-filenames -allow-lowercase -graft-points -allow-multidot -pad -r -l -J -d -v -V "SIAS" -hide-rr-moved -o SIAS-`date "+%y%m%d-%H%M%S"`.iso ISO/
```


## ISOU盘安装方法
- PMON固件
   - U盘安装
      需要选择Usb Cdrom选项进行安装，在选择此选项之后需要等待5分钟
- 昆仑固件
   - U盘安装
     1. 将机器的启动方式修改成mksh的方式
     2. 输入命令`connect`
     3. 输入命令`device`查看连接的设备
     4. 输入命令`cd fs1:`进入U盘
     5. 输入命令`cd boot`进入boot目录
     6. 输入命令`initrd initrd`加载initrd文件
     7. 输入命令`run vmlinux console=tty`启动U盘中的系统

可以依据这两个博客链接：
 1. http://blog.csdn.net/chenyulancn/article/details/16985559
 2. http://blog.csdn.net/houjian914/article/details/69817524
 3. http://www.latelee.org/using-gnu-linux/ubuntu-livecd.html
 4. https://help.ubuntu.com/community/LiveCDCustomization
  
 

