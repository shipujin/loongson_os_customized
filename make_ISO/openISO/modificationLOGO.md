### 替换F13的LOGO

1. 首先使用模糊查询到系统内包含LOGO的包、包名、位置，使用此命令：
```
yum provides */*logo*
```
输出结果为：
![输出结果](https://github.com/lina-not-linus/loongson_os_customized/blob/master/codeSnippet/logo1.png)
通过观察输出结果，可以知道fedor-logos包是fedora的logo包，知道了目标就可以准备替换图标的事情了。

2. 下载fedora-logos的RPM包，并解压出RPM包。使用命令：
```
yumdownloader fedora-logos
```
把上部下载好的fedora-logos-xxx.rpm包解开好看下内部的内容，方便之后替换。使用命令：
```
rpm2cpio fedora-logos-xxx.rpm | cpio -div
```
可以看到解压出的两个目录：/usr、/etc目录，根据之前“yum provides */*logo*”搜索结果大概知道LOGO的文件存在位置，用图形界面进入这两个目录观察，并确认
logo图片的位置。

3. 准备LOGO图标，为下一步的替换图标做准备。

4. 替换相应logo图标、图片，注意为不引起不必要的warning(所以在你不确定其他文件不引用或者不间接引用，就不要修改你要替换的文件名字).并重新压缩成tar.gz格式。

***
***

> ### 当你就这样封测成ISO并装上主机，以为你已修改了系统的LOGO,那么你就错了，因为这样安装系统后发现还是原来的样子，除了安装后启动界面的图标换成你的，其他都没有变化。

> ### 事实上你少了一部，你没有把系统LOGO的缓存文件清理，所以系统没有变化，你要把你替换图片的上一个目录里有一个“icon-theme.cache”文件，他就是系统原来图标的缓存文件，删除缓存，重新封包做ISO，装系统后会看到系统图标已变化。
