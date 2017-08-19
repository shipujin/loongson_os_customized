### yum缓存

yum缓存的好处就是你在非网络下可以安装已缓存的软件包，并解决依赖关系。
“cachedir=”为缓存地址，在“keepcache=”1为开启，0为关闭。推荐是开启前先“yum clean all”清除缓存以减少缓存大小，正常安装软件包和依赖，

已测试在安装后电脑上有缓存。位置在“/var/cache/yum/$basearch/$releasever”。

并可以使用,用以列出所有已安装的但未在yum repossitory里的安装包列表：
```
yum list extras
```

yum的缓存设置在/etc/yum.conf文件里：
```
[main]
cachedir=/var/cache/yum/$basearch/$releasever
keepcache=0
debuglevel=2
logfile=/var/log/yum.log
exactarch=1
obsoletes=1
gpgcheck=1
plugins=1
installonly_limit=3

#  This is the default, if you make this bigger yum won't see if the metadata
# is newer on the remote and so you'll "gain" the bandwidth of not having to
# download the new metadata and "pay" for it by yum not having correct
# information.
#  It is esp. important, to have correct metadata, for distributions like
# Fedora which don't keep old packages around. If you don't like this checking
# interupting your command line usage, it's much better to have something
# manually check the metadata once an hour (yum-updatesd will do this).
# metadata_expire=90m

# PUT YOUR REPOS HERE OR IN separate files named file.repo
# in /etc/yum.repos.d
```

### 安装非仓库的软件包显示信息
1. 在定制化loongson系统对系统仓库没有的软件包做迁移时，在rpm.pbone.net下载的对应的源码包，在loongson编译并打包、

安装后，安装成功。
2. 当再次进行安装时使用:
```
sudo yum install AAABBCC
```
### **竟然安装成功！！！** 

3. 然后使用查询：
```
yum info AAABBCC
```
发现仓库的来源是：
```
...
仓库：installed
...
```
4. 再次实验,移除AAABBCC包，再次查询：
```
yum info AAABBCC
```

### 安装之前未安装的仓库内的软件包：
```
sudo yum install QQWWEE
```
之后再移除
```
sudo yum remove QQWWEE
```
再次查询：
```
yum info QQWWEE
```
显示：
```
...
仓库：fedora
...
```
再次安装QQWWEE后，再次查询：
```
yum info QQWWEE

...
仓库：installed
from 仓库 @fedora
...
```
这时我好像知道了，缓存的机制了，是你在安装后的软件包都会缓存到/var/cache/yum/下目录。
