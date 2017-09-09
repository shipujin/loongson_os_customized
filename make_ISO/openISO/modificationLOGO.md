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
可以看到

3. 准备LOGO图标，为下一步的替换图标做准备。

