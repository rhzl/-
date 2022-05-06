kvm搭建入门
准备：
一台rhel八虚拟机(内存给4g最好，磁盘给40G)
1.基础配置
关闭防火墙
systemctl stop firewalld
systemctl disable firewalld
关闭selinux
vim  /etc/selinux/config

setenforce 0
查看是否支持cpu虚拟化
lsmod | grep kvm 

如果没有开就，打开虚拟机设置>把虚拟化initel VT-x/EEPT 或者 AMD-V/RVI（V）打个✔
mkdir -p /data_kvm/iso      #镜像存放地址
mkdir -p /data_kvm/node   #虚拟机存储的目录
使用xftp把iso镜像上传到 /data_kvm/iso下

搭建仓库
本地仓库
vim  /etc/yum.repos.d/file.repo
 写入配置后保存并退出
挂载镜像
[root@zl ~]# mount /dev/sr0  /mnt/
mount: /mnt: WARNING: device write-protected, mounted read-only.
1.装包
[root@zl ~]# yum install -y qemu-kvm libvirt  virt-manager  virt-install
Updating Subscription Management repositories.
Unable to read consumer identity
This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.
上次元数据过期检查：0:00:43 前，执行于 2022年04月25日 星期一 00时10分43秒。
Package qemu-kvm-15:2.12.0-63.module+el8+2833+c7d6d092.x86_64 is already installed.
Package libvirt-4.5.0-23.module+el8+2800+2d311f65.x86_64 is already installed.
Package virt-manager-2.0.0-5.el8.noarch is already installed.
依赖关系解决。
无需任何处理。
完毕！
输入命令virt-manager
就可以打开虚拟系统管理器了
双击这里
进入kvm连接详情
点击存储
创建一个名叫iso的存储然后点击前进
点击浏览选择/data_kvm/iso（这是我们之前上传了镜像的目录） 点击打开
就会出来一个叫ISO的存储池，存储池里面会有我们之前上传的镜像
然后我们再点击左下角加号创建

创建一个kvm虚拟机的存储池名字随意我的叫node(单纯觉得方便记)
然后点击前进然后点击浏览选择我们之前创建的  /data_kvm/node点击打开点击完成就会出来一个存储池

我们点击中间的加号创建一个存放kvm虚拟机的卷

创建存储卷的名字随意(我的叫node1)，格式默认qcow2，存储卷配额给10个g足够了，然后点击
完成
就会出来一个名叫node1的存储卷

创建完成后就可以点击右上角关闭就可以了
然后我们就可以开始创建虚拟机了
点击新建虚拟机然后就会出来一个窗口
选择本地安装介质(ISO映像或者光驱)(L)，点击前进
就会出来一个这样的窗口，点击浏览选择之前创建iso存储池，选择之前上传的镜像我的是Linux8.0，然后点击选择卷

然后把下面那个✔取消掉，在框内输入 /  , 选择generic(就是默认的意思),然后点击前进

接上面点击前进就会出来一个这样的窗口，内存给2048(1024也行看自己电脑配置来)
Cpu给两个（一个也行根据自己电脑配置来）然后点击前进

接上点击前进，我们在(选择或者创建自定义存储(s))这里打个✔点击管理，选择我们之前创建的存储池，选择我们创建的存储卷，点击选择卷，然后点击前进就可以了

接上面点击前进就到了这个窗口，虚拟机名字随意我的名字是node1(用户搭建环境方便记)
在安装前面自定义配置这里打个✔，然后网络不用改，默认就行，然后点击完成

点击完成后就会出来一个这样的窗口，点击引导选项，在主机引导时启动虚拟机这里打个✔，然后点击右下角应用，就可以点击左上角开始安装，kvm虚拟机就开始安装了

点击开始安装后就会出来一个这样的窗口，就是正常装虚拟机一模一样(相信大家都会我就没写了)

到这我们的kvm虚拟机就搭建完成了
