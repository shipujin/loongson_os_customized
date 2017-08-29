### 制作ISO的阶段

1. 第一个是使用通用的方法使用livecd-tools包的livecd-create和spin-kickstarts包里的ks文件，里的livecd-create工具去做ISO。
2. 第二个是使用pungi工具做F13时的做DVD的工具。
使用pungi工具链接：http://blog.csdn.net/ericzhong83/article/details/7369539
3. 第三个是由于此loongson系统仓库没有livecd-tools和spin-kickstarts的包，所以需要在rpm.pbone.net下载源码包在本龙芯机迁移编译后再使用，
需要修改livecd-create工具后自写ks打ISO。
4. 第四个也是最后一个，实在没办法做成ISO了，只能取一个没有意义的方式做ISO，直接对ISo镜像打开做修改，这个不是和ubuntu制作镜像时的方式不同。
