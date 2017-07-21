### rpm2cpio工具的使用

对于镜像列表里没有源码包，但软件仓库内有rpm二进制包：“loongson-themes”包，是对系统默认主题设置的包。
最后找到一个工具命令“rom2cpio”，用于将rpm软件包转换成cpio的文件。
```
rpm2cpio loongson-themes-版本号.rpm | cpio -div
```

现在就可以看到loongson-themes包内的文件了，打开包文件目录，看到是大部分都是标题栏的图标和壁纸。

到现在忽然想到一个文件，在想rpm算是个归档工具，我们是不是可以直接对rpm包做修改，改好后再对打开的rpm包封装成rpm包。

但是发现好像是打开的rpm包是无法再打包成rpm，恢复到原来结构，有困难。

继续研究，搜到“rpmrebuild　-ep”命令可以直接对rpm包的二进制文件起作用，对于spec文件的作用，看来还需要再对spec结构要有更深的认识。

继续查资料。

今天上午查资料终于找到了，使用“rpmrebuild　-ep”去直接修改rpm包去做包，不通过源码做修改。原理就是通过：
```
rpmrebuild -ep Package
```
直接进入了Package.spac文件，做修改后，退出，它会自动打包成rpm包。

具体过程是：

首先运行命令，
```
rpmrebuild -ep loongson-themes-1.0-2.fc13.mipsel.rpm
```
运行命令的结果：

![显示图片](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_010.png)
这个就是spac文件，其中“BuildRoot: /home/sias/.tmp/rpmrebuild.2700/work/root”是位置文件，这时

这时你会发现在“/home/sias/.tmp/rpmrebuild.2700/work/root”目录下的文件和"rpm2cpio"工具打开的rpm包的内部文件一样。

你就知道了“/home/sias/.tmp/rpmrebuild.2700/work/root”目录下就是你要修改的目录文件。

修改好后退出spac文件状态，并选择“Y”，我这里还没有修改，我就不安“Ｙ”，直接按“Ｎ”结束

![显示图片](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_011.png)

自此rpmrebuild命令直接修改rpm包，不通过源码修改的过程结束。


