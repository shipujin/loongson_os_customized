### 一. yum的主配置文件

指向仓库的位置以及相关的各种配置信息，每一个yum命令可以指向多个仓库，仓库间可以进行优先级的相关配置等，
优先级是由开销决定。

配置文件一般分为两部分组成：主配置文件和各仓库的配置文件。因为如果所有的信息都放在一个文件里那可就变得
太臃肿。其中主配置文件的路径地址在"/etc/yum.conf"，它为各仓库提供公共配置文件；各仓库的配置文件的路径地址
为"/etc/yum.repos.d/*.repo"，里面都是以赋值的格式存在。

### 二. yum主配置文件结构分析：yum.conf
```
[main]        #所有仓库的公共配置

cachedir=/var/cache/yum/$basearch/$releasevar       #缓存目录

keepcache=0       #"keepcache"变量是：程序包在安装后是否保存在缓存中，０是不保存，１是保存

debuglevel=2        #程序安装时的输出信息，数字越大输出信息越多。在生产环境中关闭最好；但在开启
                    #但是开启可以让我们快速定位安装中出现问题的所在。
                    
logfile=/var/log/yum.log        #日志文件

exactarch=1       #安装程序的版本和当前平台保持一致

obsoletes=1       #检查包已被废弃，０未被废弃

gpgcheck=1        #检查来源的合法性和包的完整性，与之对应出现的还有gpgkey，用于指明仓库的公钥文件
                  #从哪里获取，然而这个yum.conf是公共配置文件，但各个配置的仓库都不相同，所以不放在这里。
                  
plugins=1       #设置１，支持插件

installonly_limit=5       #一次安装的程序限制是５个

bugtracker_url=http://bugs.centos.org/set_project.php?                            #可以上下对比下uel
project_id=19&ref=http://bugs.centos.org/bug_report_page.php?  category=yum       #bug追踪的路径

distroverpkg=centos=release       #判定当前系统的信息


# This is the default, if you make this bigger yum won‘t see if themetadata
# is newer on the remote and so you‘ll"gain" the bandwidth of not having to
# download the new metadata and"pay" for it by yum not having correct
# information.
#  Itis esp. important, to have correct metadata, for distributions like
# Fedora which don‘t keep old packagesaround. If you don‘t like this checking
# interupting your command line usage, it‘smuch better to have something
# manually check the metadata once an hour(yum-updatesd will do this).
# metadata_expire=90m
 
# PUT YOUR REPOS HERE OR IN separate filesnamed file.repo
# in /etc/yum.repos.d    # 可以往下附加配置信息，也可以放在各配置文件中
# 可通过man yum.conf进行详细查看
```
### 下面是对比的loongsonf13的/etc/yum.conf文件结构，上下两个yum.conf文件做出对比：
```
[main]
cachedir=/var/cache/yum/$basearch/$releasever
keepcache=0
debuglevel=2
logfile=/var/log/yum.log
exactarch=1
obsoletes=1
gpgcheck=0
plugins=1
installonly_limit=3
color=never

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


