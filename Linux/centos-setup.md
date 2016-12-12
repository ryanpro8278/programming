# CENTOS的安装

## CENTOS系统简介
**CENTOS** 系统是Linux发行版之一，
它是来自于 **Red Hat Enterprise Linux** 依照开放源代码规定释出的源代码所编译而成，
此处选用该系统来演示整个安装过程。

## 安装准备
**VMWare Workstation** 、 **CENTOS** 镜像文件

## 建立虚拟机 
1. 打开VM，新建虚拟机选择 **自定义**

![](http://ww4.sinaimg.cn/large/82c8e86egw1fajej7k9nfj20dz0dewgf.jpg)

2. 按照默认点击 **下一步**

3. 选择安装来源，由于个别版本的iso镜像在此处添加后可能导致后面虚拟机安装失败，所以选择稍后安装操作系统。

![](http://ww4.sinaimg.cn/large/82c8e86egw1fajer5e1baj20dz0de3zx.jpg)

4. 选择客户机操作系统类型，版本可以根据自己下载的iso而定

![](http://ww1.sinaimg.cn/large/82c8e86egw1fajerfxdthj20dz0dedh7.jpg)

5. 选择虚拟机安装位置，选择完毕点击 **下一步**

![](http://ww2.sinaimg.cn/large/82c8e86egw1fajerqnc31j20dz0de75k.jpg)

6. 关于虚拟机的内存，建议在自己实体计算机的可用内存下使用不小于512M

![](http://ww3.sinaimg.cn/large/82c8e86egw1fajerz46nbj20dz0demyt.jpg)

7. 按照默认点击 **下一步** ，直到网络类型选用默认(NAT)

![](http://ww3.sinaimg.cn/large/82c8e86egw1fajes4chlbj20dz0de3zy.jpg)

8. 根据自己的硬盘容量设置虚拟机硬盘大小，后面可以按照默认 **下一步** 直到 **完成**

![](http://ww4.sinaimg.cn/large/82c8e86egw1fajesenis5j20dz0demyr.jpg)

## 安装系统
1. 右键单击新建好的系统，选择 **设置** ，选用已经下载好的iso镜像文件，完成后点击 **确定**

![](http://ww2.sinaimg.cn/large/82c8e86egw1fajf0bf3uyj20kf0iignz.jpg)

2. 启动虚拟机，选择第一项敲击 **Enter**

![](http://ww1.sinaimg.cn/large/82c8e86egw1fajfbhxifuj20hi098wfs.jpg)

3. 不需要对电脑媒介进行检测，选择 **skip** 跳过检测

![](http://ww3.sinaimg.cn/large/82c8e86egw1fajfcljmdmj20hs07zq3i.jpg)

4. 等待安装进度后，为了生产服务器建议安装英文版本

![](http://ww2.sinaimg.cn/large/82c8e86egw1fajfcwun3mj20hy083t9b.jpg)

5. 选用基本存储 **(Basic)** 即可

![](http://ww1.sinaimg.cn/large/82c8e86egw1fajfd54kgxj20i104zdg7.jpg)

6. 选择忽略所有数据 **(Yse, discard any data)**

7. 设置root用户名密码，提示密码太简单时可以忽略

![](http://ww4.sinaimg.cn/large/82c8e86egw1fajfe74a0hj20hw088dg9.jpg)

8. 完成后选用使用全部空间，即可完成安装