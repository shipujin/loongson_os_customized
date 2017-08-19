### 制作ISO的阶段

1. 第一个是使用通用的方法使用livecd-tools包的livecd-create和spin-kickstarts包里的ks文件，里的livecd-create工具去做ISO。
2. 第二个是使用pungi工具做F13时的做DVD的工具。
3. 第三个是由于此loongson系统仓库没有livecd-tools和spin-kickstarts的包，所以需要在rpm.pbone.net下载源码包在本龙芯机迁移编译后再使用，
需要修改livecd-create工具后自写ks打ISO。
