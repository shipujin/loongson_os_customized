### rpm2cpio工具的使用

对于镜像列表里没有源码包，但软件仓库内有rpm二进制包：“loongson-themes”包，是对系统默认主题设置的包。
最后找到一个工具命令“rom2cpio”，用于将rpm软件包转换成cpio的文件。
```
rpm2cpio loongson-themes-版本号.rpm | cpio -div
```

现在就可以看到loongson-themes包内的文件了，打开包文件目录，看到是大部分都是标题栏的图标和壁纸。

到现在忽然想到一个文件，在想rpm算是个归档工具，我们是不是可以直接对rpm包做修改，改好后再对打开的rpm包封装成rpm包。

继续研究，搜到“rpmrebuild　-pe”命令可以直接对rpm包的二进制文件起作用，对于spec文件的作用，看来还需要再对spec结构要有更深的认识。

继续查资料。

今天上午查资料终于找到了，使用“rpmrebuild　-ep”去直接修改rpm包去做包，不通过源码做修改。原理就是通过：
```
rpmrebuild -ep Package
```
直接进入了Package.spac文件，做修改后，退出，它会自动打包成rpm包。
