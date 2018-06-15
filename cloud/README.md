
# Cloud


## Tencent
- [腾讯云](https://cloud.tencent.com/)
    - [腾讯云文档平台](https://cloud.tencent.com/document/product)


#### 登录设置
- 默认支持用户名密码登录
- 可自己配置 [SSH 秘钥](https://console.cloud.tencent.com/cvm/sshkey)
    - 创建秘钥
    - 绑定/解绑云主机
    - 下载秘钥文件
    - `chmod 400 <下载的与云服务器关联的私钥的绝对路径>`
    - `ssh -i <下载的与云服务器关联的私钥的绝对路径> <username>@<hostname or ip address>`
    - 注：云服务器关联密钥后，云服务器 SSH 服务默认关闭用户名密码登录，请您使用密钥登录服务器。 

#### 其他设置
- [关于新购买Linux服务器不再提供SWAP盘的通知](https://cloud.tencent.com/document/product/362/3597)




