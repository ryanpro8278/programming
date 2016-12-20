# Git的初始设置与远程配置
## Git初始设置
* 通过[Git](https://git-scm.com/downloads)官网下载并安装Git  
* 打开 **Git Bash** 终端  
![](http://ww3.sinaimg.cn/large/82c8e86egw1faxnocrfyaj207y0bp74r.jpg)
* 在命令行输入自己的用户昵称与账户
```Bash
$ git config --global user.name "Your Name"
$ git config --global user.email "youremail@example.com"
```
* 创建 **SSH Key**
```Bash
 $ ssh-keygen -t rsa -C "youremail@example.com"
 ```
 你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。  
 如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有 `id_rsa` 和 `id_rsa.pub` 两个文件，这两个就是SSH Key的秘钥对，`id_rsa` 是私钥，不能泄露出去，`id_rsa.pub` 是公钥，可以放心地告诉任何人。
 * 进入 **GitHub** ，打开 **Account settings**，找到 **SSH Keys** 页面  
![](http://ww1.sinaimg.cn/large/82c8e86egw1faxnvh8quej207l094t90.jpg)
 * 在 **GitHub** 上添加新创建的 **SSH Key**
 填上任意Title，在Key文本框里粘贴 `id_rsa.pub` 文件的内容，点击 **Add Key** 即可成功添加
![](http://ww2.sinaimg.cn/large/82c8e86egw1faxo0symnhj20pc0lugng.jpg)
## 远程配置
* 在 **GitHub** 上建立一个新的仓库
![](http://ww3.sinaimg.cn/large/82c8e86egw1faxnj4dlppj20m30e3jta.jpg)
* 设置仓库的名称并创建  
![](http://ww2.sinaimg.cn/large/82c8e86egw1faxnkzkw5rj20m30e3mz1.jpg)
**切记：**如果我们在创建远程仓库的时候添加了README和.ignore等文件,我们在后面关联仓库后,需要先执行pull操作
* 在本地创建一个新的文件夹
![](http://ww4.sinaimg.cn/large/82c8e86egw1faxnmv1ix7j20kg01rglm.jpg)
* 打开终端 **Git Bash** ，将本地文件夹作为一个Git可以管理的仓库
```Bash
 $ git init
```
* 将本地的仓库和远程的仓库进行关联
```Bash
git remote add origin git@github.com:YotrolZ/helloTest.git
```
备注：`origin` 就是我们的远程库的名字，这是Git默认的叫法，也可以改成别的;
`git@github.com:YotrolZ/helloTest.git` 是我们远程仓库的路径，`YotrolZ/helloTest.git`要根据实际的用户名和仓库名进行修改。

