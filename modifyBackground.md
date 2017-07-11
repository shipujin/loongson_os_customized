# 对loongson系统桌面壁纸图片的修改
## 一.壁纸修改前的知识准备
对主机上安装的loongson系统文件分析，发现在"/usr/share/gnome-backgrou
gnome-backgroundgson.bz.ta

-properties/"下的xxx.xml文件，是可以修改桌面壁纸的脚本。

下面代码就是“/usr/share/gnome-background-properties/f21.xml“的xxx.xml文件：
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE wallpapers SYSTEM "gnome-wp-list.dtd">
<wallpapers>
    <wallpaper deleted="false">　　　　　　　　　　　　　　　　　　　　　　　　　　
        <name>Fedora 21 Default ‒ Phoenix</name>
        <filename>/usr/share/backgrounds/f21/default/f21.xml</filename>
        <options>stretched</options>
    </wallpaper>
</wallpapers>
```
**这个/usr/share/gnome-background-properties/下的f21.xml文件里，<wallpaper deleted="false">作用是是否在设置里显示这张壁纸。之后代码，依次往下是名字、图片位置、选项为stretched或者zoom。**

前期需要具备的修改壁纸的知识已大概了解，现准备修改源码包的内容。

## 二.正式开始对定制化壁纸修改，对源码包的修改
首先已经知道要你需要修改的目标是“桌面背景图片”，之后你通过如下命令找到包含设置背景壁纸图片的脚本文件：
```
yum provides */*background*
```
输出结果为：

图片图片

可以看出包含“gnome-background-properties/”下控制壁纸脚本的文件，所包含在哪一个包内，这时就可以从同步的源码包里
去找到包含“控制壁纸脚本文件”的包。重复第三步：

此时在makerpm的家目录，在你做的ftp服务器里复制出，包含“控制壁纸脚本文件”的包gnome-background-loongson.src.rpm包，
```
sudo scp root@192.168.30.145:/home/qwe/data/repo/fedora-source/gnome-background-loongson.src.rpm .
```
对.src.rpm包安装，依次安装出现的依赖的包，解决安装时出现的依赖的问题，
```
rpm -ivh gnome-background-loongson.src.rpm
```
会发现在rpmbuild目录下的SOUxxxx的目录下已出现gnome-background-loongson.bz.tar源码包，解压后对此包修改：
```
bz -xvf gnome-background-loongson.bz.tar
```







对解压后的源码包做修改，找到f21.xml脚本或者gnome-background相关名的脚本观察脚本内的代码，对此修改壁纸文件，当修改完后，仍压缩成
gnome-background-loongson.bz.tar文件，压缩
```

```
进入rpmbuild/SPExxx,用户用之前安装的rpmbuild工具，
```
rpmbulid -ba gnome-background-loongson.spec
```
在SRPMS目录下就出现了，你修改后的rpm背景壁纸包。






>第一步：同步fedora.repo的源，包和包的源代码，repo/source。

>第二步：对包内文件的修改。

>第三步：对修改好的源代码打成rpm包。

>第四步：建立软件仓库，creatrepo建立本地仓库。

>第五步：做成liveCD，ISO文件livecd-creator。






