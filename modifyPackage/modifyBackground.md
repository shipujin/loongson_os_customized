# 对loongson系统桌面壁纸图片的修改
## 一.壁纸修改前的知识准备
对主机上安装的loongson系统文件分析，发现在"/usr/share/gnome-backgrou
gnome-backgroundgson.bz.ta

-properties/"下的xxx.xml文件，是可以修改桌面壁纸的脚本。

下面代码就是“/usr/share/gnome-background-properties/f21.xml“的xxx.xml文件：
```xml
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

&ensp; &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; ![输出结果](http://oswj0e3on.bkt.clouddn.com/provides_background.png)


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
会发现在rpmbuild目录下的SOURCES的目录下已出现gnome-background-loongson.bz.tar源码包，与在SPECS目录下出现gnome-background-loongson.spec文件，解压SOURCES目录下的gnome-background-loongsonbz.tar包，并做出对此包修改：

解压：
```
tar xvJf gnome-background-loongson.tar.xz   //直接解压.tar.xz或者-xJf
```

对解压后的源码包做修改，找到f21.xml脚本或者gnome-background相关名的脚本观察脚本内的代码，对此修改壁纸文件。
在解压后包的目录找到gnome-background.xml文件

输出结果：

&ensp; &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; &ensp; ![结果](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground.png)

其中<filename>里的“@BACKGROUNDDIR@”的内容是图片的位置，若不知道“@BACKGROUNDDIR@”路径，可以搜索后面的“@BACKGROUNDDIR@/Blinds.jpg”图片的位置，现在的位置在gnome-background.xml位置目录
```
find . -name Blinds.jpg
```
![搜索文件](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_2.png)

则现在就可以确定“@BACKGROUNDDIR@”的意思就是和此.xml文件相同的目录路径，现在可以对包内的壁纸图片进行替换修改。

&emsp; &emsp; 当继续观察此.xml文件，发现了一个有趣的地址变量，其中一个图片地址竟然不知图片的，而是一个.xml脚本的地址。
![发现](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_3.png)

有一个壁纸路径是“@BACKGROUNDDIR@/adwaita-timed.xml”，你继续在此目录下打开/adwaita-timed.xml脚本文件，
![脚本内容](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_4.png)

其中脚本的头部标签<starttime>里是壁纸图片的生效时间，<duration>是图片的持续时间，单位是(单位/s)，<file>同样是图片的地址路径。
![脚本下面内容](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_5.png)

          




当修改完后，仍压缩成gnome-background-loongson.tar.xz文件，压缩

在SRPMS目录下就出现了，你修改后的rpm背景壁纸包。


进入rpmbuild/SPECS,用户用之前安装的rpmbuild工具，


```
tar -Jcf gnome-background-loongson.tar.xz gnome-background-loongson   //压缩.tar.xz
```
进入SPACES目录对rpmbuild对gnome-background-loongson.space文件进行
```
rpmbuild -ba gnome-background-loongson.spaces
```

则可生成rpm包，加入到软件仓库，并**更新软件仓库**
### 自此壁纸包已修改好，并加入到软件仓库，更新仓库


>第一步：同步fedora.repo的源，包和包的源代码，repo/source。

>第二步：对包内文件的修改。

>第三步：对修改好的源代码打成rpm包。

>第四步：建立软件仓库，creatrepo建立本地仓库。

>第五步：做成liveCD，ISO文件livecd-creator。






