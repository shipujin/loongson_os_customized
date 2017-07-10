# 对loongson系统桌面壁纸图片的修改
## 一.壁纸修改前的知识准备
对主机上安装的loongson系统文件分析，发现在"/usr/share/gnome-background-properties/"下的xxx.xml文件，是可以修改桌面壁纸的脚本。

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
这个/usr/share/gnome-background-properties/下的f21.xml文件里，<wallpaper deleted="false">作用是是否在设置里显示这张壁纸。之后代码，依次往下是名字、图片位置、选项为stretched或者zoom。

前期需要具备的修改壁纸的知识已大概了解，现准备修改源码包的内容。

## 二.正式开始对定制化壁纸修改，对源码包的修改
首先已经知道要你需要修改的目标是“桌面背景图片”，之后你通过如下命令找到包含设置背景壁纸图片的脚本文件：
```
yum provies */*background*
```
输出结果为：

图片图片

可以看出包含“gnome-background-properties/”下控制壁纸脚本的文件，所包含在哪一个包内，这时就可以从同步的源码包里
去找到包含“控制壁纸脚本文件”的包，








