# 对loongson系统桌面壁纸图片的修改
## 一.壁纸修改前的知识准备
对主机上安装的loongson系统文件分析，发现在"/usr/share/gnome-background-properties/"下的xxx.xml文件，是可以修改桌面壁纸的脚本。

下面代码就是“/usr/share/gnome-background-properties/xxxx.xml“的xxx.xml文件：

![显示](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_03.png)
其中“/usr/share/”下的“background/”目录下是存放壁纸的目录，但其中也有XXX.xml文件，在下面也会介绍。

以desktop-backgrounds-basic.xml举例说明，
“/usr/share/gnome-background-properties/desktop-backgrounds-basic.xml“的各个xml脚本文件，格式都是已下结构：

![显示](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_01.png)
![显示](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_02.png)

这里要说明，里面的大部分都是各个国家语言的图片壁纸命名的名字。

脚本文件里的区别就是壁纸存放的位置或者是嵌套着.xml脚本，

对于这样嵌套的.xml例如："/usr/share/gnome-background-properties/desktop-backgrounds-goddard.xml"代码如下：

![显示图片](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_04.png)

继续深入到脚本内的.xml的路径：
![显示图片](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_05.png)

显而易见这是一个分辨率不同的同一张图片，<starttime>标签内是壁纸生效时间，<duration>是生效的时间，
一般用的壁纸是很久的，所以会设置一个大的值，其中的单位一样的s(秒)

例如：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE wallpapers SYSTEM "gnome-wp-list.dtd">
<wallpapers>
    <wallpaper deleted="false">　　　　　　　　　　　　　　　　　　　　　　　　　　
        <name>Fedora 21 Default ‒ Phoenix</name>
        <filename>/usr/share/backgrounds/XXXX/xxxx.xml</filename>　//目录里的文件结构都是如下结构，区别就是name与壁纸保存的位置不同，
        <options>stretched</options>　　　　　　　　　　　　　　　　　　//和其中设置的脚本不同，与位置
    </wallpaper>
</wallpapers>

```
**这个/usr/share/gnome-background-properties/下的f21.xml文件里，<wallpaper deleted="false">作用是是否在设置里显示这张壁纸。之后代码，依次往下是名字、图片位置、选项为stretched或者zoom。**

前期需要具备的修改壁纸的知识已大概了解，现准备修改源码包的内容。

## 二.正式开始对定制化壁纸修改，对源码包的修改
首先已经知道要你需要修改的目标是“桌面背景图片”，之后你通过如下命令找到包含设置背景壁纸图片的脚本文件：
```
yum provides */*desktop-backgrounds-*
```

输出结果为：

![显示图片](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_06.png)

可以看出包含“*/*desktop-backgrounds-*”此关键词的文件的包，所包含在哪一个包内，这时就可以从同步的源码包里
去找到包含“控制壁纸脚本文件”的包。重复第三步：

此时在makerpm的家目录，在你做的ftp服务器里复制出，包含“控制壁纸脚本文件”的源码包desktop-backgrounds-basic-9.0.0-14.fc13.noarch.src.rpm包，
```vim
sudo scp root@192.168.30.145:/home/qwe/data/repo/fedora-source/desktop-backgrounds-basic-9.0.0-14.fc13.noarch.src.rpm . //这里还有“.,此目录”
```

对.src.rpm包安装，依次安装出现的依赖的包，解决安装时出现的依赖的问题，

```vim
rpm -ivh desktop-backgrounds-basic-9.0.0-14.fc13.noarch.src.rpm
```

会发现在rpmbuild目录下的SOURCES的目录下已出现gnome-background-loongson.bz.tar源码包，与在SPECS目录下出现gnome-background-loongson.spec文件，解压SOURCES目录下的gnome-background-loongsonbz.tar包，并做出对此包修改：

解压：
```vim
tar -jxvf redhat-backgrounds-15.tar.bz2 
```

对解压后的源码包做修改，找到desktop-backgrounds-basic.xml脚本或者desktop-backgrounds相关名的脚本观察脚本内的代码，对此修改壁纸文件。
在解压后包的目录找到desktop-backgrounds相关的.xml文件

输出结果：

![图片显示](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_01.png)
![图片显示](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_02.png)

可以看到在包里看到的desktop-backgrounds-basic.xml脚本文件，和之前在安装的本地系统上的文件一样内容，与格式。

为了找到在xxxx.xml文件里的图片，有时图片的路径是从/usr/share/desktop-backgrpund/开头的，所以你这时看着路径有点蒙。

那么你可以用到这个find命令去寻找壁纸图片的位置
```
find . -name xxxx.jpg
```
![搜索文件](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_2.png)

则现在就可以知道壁纸图片的在包里的位置了，方便你去做自己的壁纸图片。

&emsp; &emsp; 当继续观察此.xml文件，发现了一个有趣的地址变量，其中一个图片地址竟然不知图片的，而是一个.xml脚本的地址。
![发现](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_3.png)

有一个壁纸路径是“@BACKGROUNDDIR@/adwaita-timed.xml”，你继续在此目录下打开/adwaita-timed.xml脚本文件，
![脚本内容](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_4.png)

其中脚本的头部标签<starttime>里是壁纸图片的生效时间，<duration>是图片的持续时间，单位是(单位/s)，<file>同样是图片的地址路径。
![脚本下面内容](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_5.png)

          




当修改完后，仍压缩成redhat-backgrounds-15.tar.bz2文件，压缩
```
tar -jcvf redhat-backgrounds-15.tar.bz2
```

进入SPACES目录对rpmbuild对desktop-backgrounds.spec文件进行
```
rpmbuild -ba desktop-backgrounds.spec
```

在RPMS目录下发现三个包，如下：
![显示图片](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_07.png)

#### 现在出现了问题：

 ~~我修改一个A源码包，产生三个二进制包A,B,C，放入我的软件仓库，并更新，当我再次下载Ｂ做修改---->推测修改后还是，三个包，想问　两次的ＡＡ　　与未修改的ＢＢ　　有变化吗
如何消除这个困难？对三个源码包的修改~~








则可生成rpm包，加入到软件仓库，并**更新软件仓库**
### 自此壁纸包已修改好，并加入到软件仓库，更新仓库


>第一步：同步fedora.repo的源，包和包的源代码，repo/source。

>第二步：对包内文件的修改。

>第三步：对修改好的源代码打成rpm包。

>第四步：建立软件仓库，creatrepo建立本地仓库。

>第五步：做成liveCD，ISO文件livecd-creator。









