### 一. spec文件解析

例子：这是 Fedora 16 eject 程序的 spec 文件：取自fedora手册https://fedoraproject.org/wiki/How_to_create_an_RPM_package/zh-cn
```
vi python-antigravity.spec

Summary:            A program that ejects removable media using software control
Name:               eject
Version:            2.1.5
Release:            21%{?dist}
License:            GPLv2+
Group:              System Environment/Base
Source:             %{name}-%{version}.tar.gz
Patch1:             eject-2.1.1-verbose.patch
Patch2:             eject-timeout.patch
Patch3:             eject-2.1.5-opendevice.patch
Patch4:             eject-2.1.5-spaces.patch
Patch5:             eject-2.1.5-lock.patch
Patch6:             eject-2.1.5-umount.patch
URL:                http://www.pobox.com/~tranter
ExcludeArch:        s390 s390x
BuildRequires:      gettext
BuildRequires:      libtool

%description
The eject program allows the user to eject removable media (typically
CD-ROMs, floppy disks or Iomega Jaz or Zip disks) using software
control. Eject can also control some multi-disk CD changers and even
some devices' auto-eject features.

Install eject if you'd like to eject removable media using software
control.

%prep
%autosetup -n %{name}

%build
%configure
make %{?_smp_mflags}

%install
%make_install

install -m 755 -d %{buildroot}/%{_sbindir}
ln -s ../bin/eject %{buildroot}/%{_sbindir}

%find_lang %{name}

%files -f %{name}.lang
%doc README TODO COPYING ChangeLog
%{_bindir}/*
%{_sbindir}/*
%{_mandir}/man1/*

%changelog
* Tue Feb 08 2011 Fedora Release Engineering <rel-eng@lists.fedoraproject.org> - 2.1.5-21
- Rebuilt for https://fedoraproject.org/wiki/Fedora_15_Mass_Rebuild

* Fri Jul 02 2010 Kamil Dudka <kdudka@redhat.com> 2.1.5-20
- handle multi-partition devices with spaces in mount points properly (#608502)
```
以下是简单介绍：
```
Summary:  名字的全程、描述该软件的用途和特性

Name:     包名

Version:  版本号，官方版本号、上游版本号

Release：  发行编号。初始值为1%{?dist}，每次制作新包需要递增这个数字

License:  授权协议类型，用来表示该软件采用的哪一种授权协议类型

URL:      该软件的项目主页，源码包的URL，请使用Source0指定(source0关键字在下面会介绍到)

Source0:  软件源码包的URL地址，建议使用%{name}和%{version}替换URL中的名称/版本

Patch0:   用于对源码的补丁名称，patch放在和源码的目录同，都在~/rebuild/SOURCE/下

BuildArch:用于表示此软件依赖的架构，若你要打包的文件不依赖任何架构(例如shell、数据文件)，就需要写为"BuildArch: noarch"。rpm架构会变成"noarch"。

BuildRoot:在%install阶段(%build阶段之后)，文件安装到此位置。Fedora不需要此标签，EPEL5还需要它。默认情况下，根目录为: %{_topdir}/BUILDROOT/

BuildRequires:编译软件包所需要的依赖包列表，以逗号分隔

Requires:     安装软件包所需要的依赖包列表，以逗号分隔

%description: 程序的详细描述，使用美式英语，每行小于80字符，空行为新段落

%prep:        打包准备阶段执行的一些命令，例如解压源码包、打补丁等，以便开始编译。一般来说仅包含“%autosetup”,如果源码包需要解压并切换至NAME目录，则需要输入“%autosetup -n NAME”

%build：       包含构建阶段执行的命令，构建完成后就开始后续的安装。程序需要包含有如何编译的介绍。

%install：     包含安装阶段执行的命令，命令从%{_builddir}目录安装到%{buildroot}目录

%check:        包含测试阶段执行的命令。此阶段在%install阶段之后执行，一般包含“make test”或“make check”命令。此阶段要与%build分开，以便在需要时忽略测试

%clean：        清理安装目录的命令。此阶段是在fedora中是多余的，仅针对EPEL。通常仅包含： rm -rf %{buildroot}


%file:          需要被打包/安装的文件列表

%changelog:     rpm变更日志。但需要注意的是，不是软件本身的变更日志

%ExcloudeArch:  排除某些架构。如果该软件不能在某些架构上正常编译或者安装，可以使用此标签排除某些架构

%ExclusiveArch: 列出该软件包独占的架构





%prep:        打包准备阶段执行的一些命令、解压源码包、打补丁，以便开始接下来的编译
```
依据fedora手册学习打包与自己写spec文件链接：https://fedoraproject.org/wiki/How_to_create_an_RPM_package/zh-cn
