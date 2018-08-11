
# Cloud

## Tencent
- [腾讯云](https://cloud.tencent.com/)
    - [腾讯云文档平台](https://cloud.tencent.com/document/product)


## 登录设置
#### SSH 登录
- 默认支持用户名密码登录
- 可自己配置 [SSH 秘钥](https://console.cloud.tencent.com/cvm/sshkey)
    - 创建秘钥
    - 绑定/解绑云主机
    - 下载秘钥文件
    - `chmod 400 <下载的与云服务器关联的私钥的绝对路径>`
    - `ssh -i <下载的与云服务器关联的私钥的绝对路径> <username>@<hostname or ip address>`
    - 注：云服务器关联密钥后，云服务器 SSH 服务默认关闭用户名密码登录，请您使用密钥登录服务器。 

#### 多用户登录
- [不同用户使用各自的SSH密钥登录](http://wsq.discuz.qq.com/?c=index&a=viewthread&f=inner&tid=12712&siteid=264281419)
- [ubuntu 新建用户到指定的目录](https://blog.csdn.net/baidu_35679960/article/details/78752591)
- [设置 SSH 通过密钥登录](https://hyjk2000.github.io/2012/03/16/how-to-set-up-ssh-keys/)
- [linux 系统中 /etc/passwd 和 /etc/shadow文件详解](https://blog.csdn.net/yaofeino1/article/details/54616440)

相关文件
- `/etc/sudoers` su、sudo 权限控制
- `/etc/passwd` 保存用户
- `/etc/shadow` 保存用户对应的密码

#### SWAP 分区
- [关于新购买Linux服务器不再提供SWAP盘的通知](https://cloud.tencent.com/document/product/362/3597)


## 遇到的问题
#### 腾讯云默认不再提供 SWAP 区，需要自己设置
- [关于新购买Linux服务器不再提供SWAP盘的通知](https://cloud.tencent.com/document/product/362/3597)
- [云服务器 ECS Linux SWAP 配置](https://cloud.tencent.com/info/8dc52da899e2d9d31853413d594becea.html)
- [对于/etc/fstab的解释及修改文件系统的label](https://blog.csdn.net/avilifans/article/details/11848305)

**Example**：分配 2G SWAP 区：
- `$ free -m` 查看系统当前的分区情况
- `$ swapon -s` or `$ swapon --show` 查看当前 SWAP 区信息
- `$ df -h` 查看当前磁盘的使用情况
- `$ sudo dd if=/dev/zero of=/opt/swap bs=1M count=2048` 创建用于交换分区的文件
- `$ sudo mkswap /opt/swap` 设置交换分区文件
- `$ sudo swapon /opt/swap` 立即启用交换分区文件
- `$ sudo vim /etc/fstab` 追加 `LABEL=SWAP-sda /opt/swap swap swap defaults 0 0` 设置开机时自启用 SWAP 分区
- `$ cat /proc/sys/vm/swappiness` 查看当前 swpapiness 参数
    - `swappiness` 参数配置您的系统将数据从RAM交换到交换空间的频率, 值介于0和100之间，表示百分比
- `$ sudo sysctl vm.swappiness=10` 修改 swpapiness 参数，仅当前生效
- `$ sudo vim /etc/sysctl.conf` 追加 `vm.swappiness=10` 设置永久生效的 swappiness 参数

关闭 SWAP 操作：
- `$ swapoff /mnt/swap` 当前关闭
- `$ vim /etc/fstab` 注释前面添加的内容




