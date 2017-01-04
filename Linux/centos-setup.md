# CENTOS的安装

## 简介
**CENTOS** 系统是Linux发行版之一，
它是来自于 **Red Hat Enterprise Linux** 依照开放源代码规定释出的源代码所编译而成，
此处选用该系统来演示整个安装过程。

## 环境
**VMWare Workstation**   

**CENTOS** 镜像文件

## 步骤 
### 1 建立虚拟机

a. 打开VM，新建虚拟机选择 **自定义**  
![](http://ww4.sinaimg.cn/large/82c8e86egw1fajej7k9nfj20dz0dewgf.jpg)

b. 按照默认点击 **下一步**

c. 选择安装来源，由于个别版本的iso镜像在此处添加后可能导致后面虚拟机安装失败，所以选择稍后安装操作系统  
![](http://ww4.sinaimg.cn/large/82c8e86egw1fajer5e1baj20dz0de3zx.jpg)

d. 选择客户机操作系统类型，版本可以根据自己下载的iso而定  
![](http://ww1.sinaimg.cn/large/82c8e86egw1fajerfxdthj20dz0dedh7.jpg)

e. 选择虚拟机安装位置，选择完毕点击 **下一步**  
![](http://ww2.sinaimg.cn/large/82c8e86egw1fajerqnc31j20dz0de75k.jpg)

f. 关于虚拟机的内存，建议在自己实体计算机的可用内存下使用不小于512M  
![](http://ww3.sinaimg.cn/large/82c8e86egw1fajerz46nbj20dz0demyt.jpg)

g. 按照默认点击 **下一步** ，直到网络类型选用默认(NAT)  
![](http://ww3.sinaimg.cn/large/82c8e86egw1fajes4chlbj20dz0de3zy.jpg)

h. 根据自己的硬盘容量设置虚拟机硬盘大小，后面可以按照默认 **下一步** 直到 **完成**  
![](http://ww4.sinaimg.cn/large/82c8e86egw1fajesenis5j20dz0demyr.jpg)

### 2 安装系统

a. 右键单击新建好的系统，选择 **设置** ，选用已经下载好的iso镜像文件，完成后点击 **确定**  
![](http://ww2.sinaimg.cn/large/82c8e86egw1fajf0bf3uyj20kf0iignz.jpg)

b. 有时候会显示下图所示的错误，这个时候需要进入主机的 **BIOS** 将 **Intel HT Technology** 改为 **Enable**状态  
![](http://ww1.sinaimg.cn/large/82c8e86egw1faqnck2humj20fk089q4i.jpg)

c. 启动虚拟机，选择第一项敲击 **Enter**  
![](http://ww4.sinaimg.cn/large/82c8e86egw1faqnegkwkyj20ht0dcgo1.jpg)

d. 不需要对电脑媒介进行检测，选择 **skip** 跳过检测  
![](http://ww3.sinaimg.cn/large/82c8e86egw1fajfcljmdmj20hs07zq3i.jpg)

e. 等待安装进度后，为了生产服务器建议安装英文版本  
![](http://ww2.sinaimg.cn/large/82c8e86egw1fajfcwun3mj20hy083t9b.jpg)

f. 选用基本存储 **(Basic)** 即可  
![](http://ww1.sinaimg.cn/large/82c8e86egw1fajfd54kgxj20i104zdg7.jpg)

g. 选择忽略所有数据 **(Yse, discard any data)**  
![](http://ww1.sinaimg.cn/large/82c8e86egw1faqngw5dgkj20m90gr40g.jpg)

h. 设置root用户名密码，提示密码太简单时可以忽略 **(Use it anyway)**  
![](http://ww1.sinaimg.cn/large/82c8e86egw1faqni3tb3hj20ma0godh0.jpg)

i. 选择 **Creaet Custom Layout**，如果是虚拟机省时可以直接选择 __Use All Space__  
![](http://ww1.sinaimg.cn/large/82c8e86egw1faqnkvunznj20m70glacd.jpg)

### 3 系统分区

a. 选择 **Create**进行安装，采用标准分区  
![](http://ww1.sinaimg.cn/large/82c8e86egw1faqow8cvs4j20lx0f9q4j.jpg)

b. 建立/boot分区，选用ext3格式，大小100M-200M即可  
![](http://ww1.sinaimg.cn/large/82c8e86egw1faqow8cvs4j20lx0f9q4j.jpg)

c. 创建swap类型的分区，这个分区是在内存空间不足时充当设备内存使用的，大小是系统内存的一到两倍左右。  
![](http://ww4.sinaimg.cn/large/82c8e86egw1faqpk8l5qbj20e20d1mya.jpg)

d. 创建系统的根分区，选用ext4格式，大小选用剩余所有空间 **fill to maximum allowable size**  
![](http://ww4.sinaimg.cn/large/82c8e86egw1faqpkqgvhdj20e30d03zn.jpg)  
**(用于正式生产的服务器，切记必须把数据盘单独分区，以保证数据的完整性。比如可以再划分一个/data专门用来存放数据)**

e. 完成上一步后点击 **Next**，选择 **Format**进行格式化，然后选择 **Write changes to disk**  
![](http://ww3.sinaimg.cn/large/82c8e86egw1faqpl93x56j20a804h0t6.jpg)

f. 下一步是选择启动程序的位置，按照默认即可，紧接着自定义安装，为了便于Windows用户熟悉该系统选择 **Desktop**  
![](http://ww3.sinaimg.cn/large/82c8e86egw1faqplgsi42j20m40gbdho.jpg)

+ 重启即可完成安装

![](http://ww2.sinaimg.cn/large/82c8e86egw1faqpo82depj20qw0hs400.jpg)



参考:[http://www.jb51.net/os/128751.html](http://www.jb51.net/os/128751.html)