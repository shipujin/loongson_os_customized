### 按照fedora手册去做系统发行版：去做龙芯F13的系统定制化

* 首先你需要同步仓库

首先我去同步仓库，同步二进制、源代码仓库，发现同步完仓库后，只有6G多，不可思议，这时我就感到不对劲，因为我之前同步龙梦10时的仓库有60多G，但我当时没多想，就继续往下做。

* 从源码重新编译rpm包

从源码重新编译rpm包，当我重新编译完logo包、background-desktop包，继续想在同步的源码包仓库找loongson-themes源码包(因为我之前在系统上查询过，系统之前装过
loongson-themes包)，但发现同步的源码包仓库里没有loongson-themes源码包，我以为我同步时没有同步完全就去镜像列表的源码包目录找loongson-themes源码包，
然而源码包目录并没有此包。

为了验证此包，就去看二进制目录里找loongson-themes包，发现竟然有loongson-themes二进制包！！！

但没有loongson-themes源码包，奇怪、奇怪！！！

> #### 我当时就觉得他们由于一些原因，并没有把全部的源码包放出来。

但还定制化还是需要进行下去。

当时我想RPM包就是一种封装方式，所以就去网上去查直接对RPM包做修改。
直接对RPM包做修改，研究如下：rpmrebuile工具直接对RPM修改![rpmrebuile工具直接对RPM修改](https://github.com/lina-not-linus/loongson_os_customized/blob/master/openRPMmodify/openRPM.md)

* 并迁移tmiux工具，去拿tmux源码包在龙芯机上编译生成二进制文件。

* 制作ISO镜像

当做的差不多桌面环境的修改后，开始准备ISO的制作，当我去fedora手册上记录的方法安装：livecd-tools、spin-kickstarts这两个工具时，发现仓库里没有这两个工具，需要去下载源码包在龙芯机上编译，需要自己解决依赖问题，当我解决依赖后，发现livecd-create使用也有问题，当解决使用问题，开始写ks文件，都是坑，我就自己写，搜索系统自己安装的所以RPM包，输入到ks文件里，发现仓库里根本就没有里面的相当一部分包。

#### fedora手册方法制作ISO，失败

### 仿照Ubuntu系统制作ISO的方式做ISO：直接拆ISO，改完再恢复到做成ISO




