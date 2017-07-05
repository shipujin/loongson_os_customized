　<系统定制化计划>

第一步：同步fedora.repo的源，包和包的源代码，repo/source

第二步：对包内文件的修改

第三步：对修改好的源代码打成rpm包

第四步：建立软件仓库，creatrepo建立本地仓库

第六步：做成liveCD，ISO文件livecd-creator




以下是详细过程：

第一步同步软件仓库，软件包和源码包：

yum-utils //安装这个包是为了用到里面的工具“reposync”

reposync -h //查看reposync 工具的帮助查看各个参数


## 同步软件仓库

reposync -r fedora -p ./  //同步编译好的软件包

reposync -r fedora-source --source  //同步软件包的源代码



## 建立FTP服务器把将近１００Ｇ的文件传送到FTP

ip a  //因未安装net-tools，所以无ifconfig命令　

scp lhosts root@192.168.30.233:~  //把目录下的lhosts发送到ip192.168.30.233的主机的root用户家目录

sudo scp -r fedora/ 192.168.30.142:/home/qwe/data/repo  //同上，把一个目录传到ip192.168.30.233的主机/home下的/qwe/data/repo目录

yumdownloader　packageName   //用yumdownloader命令下载packageName

rpm.pbone.net   //若有未同步下来的包，在rpm.phone.net网站搜索下载



## mock编译工具

1. useradd mockbuilder   //新建一用户，作为编译用户

2. usermod -a -G mock mockbuilder     //并放进mock用户组

3. su - mockbuilder    //注意：如果强行使用root身份执行mock命令，python将会抛出“RuntimeError: mock will not run from the root account”的错误。


4. mock的配置文件在/etc/mock/目录下，系统默认内置了fedora,epel-等系统的配置文件，通过查看这些.cfg的配置文件，可以了解mock的原理，

5. 但现在我在龙梦的live-10系统/etc/mock/目录没有看到关于loongson龙芯的相关配置－－>>所以龙芯机没办法使用mock工具编译

###### 例：假如编译一个Fedora15 64位系统的软件包来举例（则对应的配置文件为：fedora-15-x86_64.cfg），相关的命令是：

1. #mock -r fedora-15-x86_64 --init   //初始化，不需要加.cfg后缀

2. #rebuild package-1.2-3.src.rpm     //开始编译过程

3. 初始化以后的最小化系统环境位于：/var/lib/mock/fedora-15-x86_64/root目录下，可以用chroot命令切换此最小化环境。

4. 编译后的日志和软件包会保存在：/var/lib/mock/fedora-15-x86_64/result。




## 第三步对源码包打成rpm包

### <使用rpmbuild编译打包>

1. 首先设置用户，

2. 你要编译打包哪个包，就先安装哪个包例如源码包“a2jmidid-8-8.fc21.src.rpm”

3. 在编译用户下，安装“a2jmidid-8-8.fc21.src.rpm”，rpm -ivh a2jmidid-8-8.fc21.src.rpm

4. 安装后，发现打开“rpmbuild/SPECS/”目录出现“a2jmidid.spec”，在"rpmbuild/SOURCES/"目录里是源码文件，可修改它们对所要做的系统定制化做修改。

5. 现在编译“a2jmidid.spec”，执行命令“rpmbuild -ba a2jmidid.spec”
