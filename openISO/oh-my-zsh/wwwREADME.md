### 设置默认shell为zsh

> #### 以下的工作环境都为：解压的Fedora13-Desktop-mipso32-release-20160112.tar.gz目录下，使用chroot把解压后的目录作为工作目录

一.首先设置useradd时默认是zsh，修改系统配置文件：
```
首先安装好zsh: yum install zsh
vim /etc/default/useradd
把 “SHELL=/bin/bash”
改为 “SHELL=/bin/zsh”
```
现在系统创建的用户都默认使用的是zsh

二.配置zsh，增加zsh的种种方便的功能

使用![oh-my-zsh的github项目](https://github.com/robbyrussell/oh-my-zsh)，相关实现设置：

1. 首先从github上下载下来oh-my-zsh的整个项目oh-my-zsh,

2. 然后把项目修改为隐藏文件：.oh-my-zsh，并把.oh-my-zsh移到/etc下：mv .oh-my-zsh etc/

3. 然后把.oh-my-zsh/templates/zshrc.zsh-template 复制到etc/zshrc系统配置文件，并修改文件内容：把“export ZSH=$home/.oh-my-zsh”的zsh配置文件和插件的
位置地址换成“export ZSH=/etc/.oh-my-zsh”

> 可以把工作目录重新压缩成Fedora13-Desktop-mipso32-release-20160112.tar.gz了，并打成ISO文件。

