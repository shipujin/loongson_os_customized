### 一. 什么是repo文件

repo文件是Fedora中的yum源软件仓库的配置文件，通常一个repo文件定义了一个或者多个软件fedorassss仓库
的细节内容，例如我们将从哪里下载需要安装或者升级的软件包，repo文件中的设置内容将被yum读取
和应用。

### 二. yum源repo文件的格式
```vim
[fedora]        #方括号里面的是软件源的名称，将被yum读取并识别

name=Fedora $releasever - $basearch       #这里也是定义了软件仓库的名字：Fedora，
                                          #后面的$releasever变量定义了发行版号，通常是
                                          #是数字，$basearch变量定义了系统的架构，可以是
                                          #是i386、x86_64、ppc等架构名值，这两个变量
                                          #根据系统的版本、架构不同而有不同的取值，这可以方便
                                          #方便yum升级的时候选择合适当前系统的软件包。
                                         
failovermethod=priority       #关键字"failovermethod"的中文是“失效备援”见名知意
                              #后面可以有两个选项，“priority”与“roungrobin”的
                              #这两个选项。；其中一个是priority是默认值值，是在baseurl中
                              #“顺序选择”镜像服务器地址；另一个是roundrobin表示在
                              #在baseur“随机选择”镜像服务器地址
                              

exclude=compiz* *compiz* fusion-icon*       #“exclude”排除；排斥；拒绝接纳；逐出
                                            #排除；排斥；拒绝接纳；逐出的意思，
                                            #是用来禁止软件仓库中的某些软件包的安装和更新，
                                            #可以使用通配符，并且用空格分开，根据自己的
                                            #实际情况使用此变量参数
                                            

#下面的制定镜像服务器镜像列表可以有两种方式：

#1：指定一个basearch源的镜像服务器地址
baseurl=http://mirrors.aliyun.com/fedora/releases/$releasever/Everything/$basearch/os/ 

#2：此一般为默认列表，指定一个镜像服务器地址列表，对于更加好的认识"$releasever"与"$basearch"两个变量
mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=fedora-$releasever&arch=$basearch

#   将"$releasever"与"$basearch"替换成自己对应的版本与架构，例如10和x86_64，在浏览器
#   打开，就可以看到一列的镜像服务器地址列表。
#　　在这一列的镜像服务器地址复制到repo文件中，这时我们就可以获得更加快速的更新速度，加
#   入到baseurl=内：

enabled=1       #设置为１表示repo文件定义的的源是启用的，０为禁用定义的源。

metadata_expire=7d        #默认带着就好，缓存存放时间，７d－>7天

gpgcheck=1        #gpgcheck项表示这个repo中下载的rpm包进行gpg的校验，为了确定rpm包的来源是有效的和安全的
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$basearch       #定义用于校验的gpg密钥


#repo文件后面下面的内容
[fedora-debuginfo] 
name=Fedora $releasever - $basearch - Debug - aliyun
failovermethod=priority 
baseurl=http://mirrors.aliyun.com/fedora/releases/$releasever/Everything/$basearch/debug/ 
#mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=fedora-debug-$releasever&arch=$basearch 
enabled=0 
metadata_expire=7d 
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$basearch 
 
[fedora-source]       #源码包
name=Fedora $releasever - Source - aliyun
failovermethod=priority 
baseurl=http://mirrors.aliyun.com/fedora/releases/$releasever/Everything/source/SRPMS/ 
#mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=fedora-source-$releasever&arch=$basearch 
enabled=0 
metadata_expire=7d 
gpgcheck=1 
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$basearch 


```

### 下面是loongson的/etc/yum.repos.d/fedora.repo配置文件，做出上下的XXX.repo文件的结构对比：
```
[fedora]
name=Fedora $releasever - $basearch
enable=1
gpgcheck=0
baseurl=http://ftp.loongnix.org/os/Fedora13-o32/RPMS/
        http://10.2.5.28/ftp/download/os/Fedora13-o32/RPMS/

[updates]
name=Fedora $releasever - $basearch
enable=1
gpgcheck=0
baseurl=http://ftp.loongnix.org/os/Fedora13-o32/Updates/RPMS/
        http://10.2.5.28/ftp/download/os/Fedora13-o32/Updates/RPMS

```

由于此没有feodra-source的repo，决定自己写一个repo文件，通过[fedoea]、[update]的baseurl的地址路径写一个source的repo文件：

所要加的格式内容：
```
[fedora-source]
name=Fedora $releasever - $source
enable=1
gpgcheck=0
baseurl=http://ftp.loongnix.org/os/Fedora13-o32/SRPMS/
        http://10.2.5.28/ftp/download/os/Fedora13-o32/SRPMS/
```
发现还是不能够同步source源码包，并且yum源并爆出错误警告，打开source的地址http://ftp.loongnix.org/os/Fedora13-o32/SRPMS/
后发现，没有repodate文件目录，所以无法同步,还是修改什么，再wget源码包，地址还是用source上的地址。

### 三. 更多的使用：man yum.conf，一切以man为准。
