# 经常使用的命令汇总

1. 搜索软件是否安装：
```
rpm -qa Package名
```

2. 搜索Package包：
```
yum search Package
```

3. 显示包的相关信息:
```
yum info Package
```

4. 只下载Package包，不安装：
```
yumdownloader Package
```

5. 打开rpm包：
```
rpm2cpio Package | cpio -dio
```

6. 搜索某个文件在哪一个rpm包内：
```
yum provides Package
```
这样就可以通过找到的包去下载源码包去搞事情了。
