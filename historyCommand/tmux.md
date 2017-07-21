# 使用tmux工具防远程中断

放一张正在使用的tmux，为了防止远程连接的龙芯机远程中断，造成正在编译的包或者工作废弃。

![显示图片](http://oswj0e3on.bkt.clouddn.com/loongson_os_customized/codeSnippet/provides_background_gnomebackground_09.png)

### 首先对于配置文件的设置，系统配置/etc/tmux.conf文件，本用户配置~/.tmux.conf
```
#设置命令前缀健，为Ctrl-a
set -g prefix C-a       //-g是全局变量配置

#把命令前缀健释放，
unbind C-b        //

#对~/.tmux.conf文件加入的内容配置，可以立即生效
bind R source-file ~/.tmux.conf ; display-message "Config reloaded"
```

### 使用tmux
```
tmux new -s XXX                 //新建一个tmux，名字叫XXX
exit或者tmux kill-session -t XXX //在tmux内时，用exit退出此个tmux
tmux ls                         //列出存在的各tmux
tmux attach -t XXX              //进入一个名字是XXX的tmux
```

### 使用tmux时的各快捷健
```
C-a %         --->水平分屏，平均
C-a "         --->（一个双引号）上下分屏，平均
C-a 上下左右健 --->进入你分的各个屏
```
