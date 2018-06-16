# GitLab
GitLab 目前的功能已经非常强大，足以支持团队开发，对于一般用户，CE（Community Edition） 版本已经够用；
- [GitLab](https://about.gitlab.com/)


## 基于 Docker 安装 GitLab
#### 使用 `docker-compose` 安装 
- [GitLab Docker images](https://docs.gitlab.com/omnibus/docker/)

`docker-compose.yml` 文件如下：
```
web:
  container_name: docker-gitlab-web
  image: 'gitlab/gitlab-ce:latest'
  restart: always
  hostname: 'gitlab.example.com'
  environment:
    GITLAB_OMNIBUS_CONFIG: |
      external_url 'http://gitlab.example.com'

      # backup config. 30 days.
      gitlab_rails['backup_keep_time'] = 2592000

      # smtp config. 163
      gitlab_rails['smtp_enable'] = true
      gitlab_rails['smtp_address'] = "smtp.163.com"
      gitlab_rails['smtp_port'] = 25
      gitlab_rails['smtp_user_name'] = "test@163.com"
      gitlab_rails['smtp_password'] = "test_pw"
      gitlab_rails['smtp_domain'] = "163.com"
      gitlab_rails['smtp_authentication'] = "login"
      gitlab_rails['smtp_enable_starttls_auto'] = true
      gitlab_rails['smtp_tls'] = false
      # config sender
      gitlab_rails['gitlab_email_from'] = "test@163.com"
      user['git_user_email'] = "test@163.com"

      # Add any other gitlab.rb configuration here, each on its own line
  ports:
    - '20443:443'
    - '2080:80'
    - '2022:22'
  volumes:
    - '/srv/gitlab/config:/etc/gitlab'
    - '/srv/gitlab/logs:/var/log/gitlab'
    - '/srv/gitlab/data:/var/opt/gitlab'
    - '/srv/gitlab/backups:/var/opt/gitlab/backups'
```

其中 `GITLAB_OMNIBUS_CONFIG` 用于自定义 GitLab 默认配置，配置项见：[Omnibus GitLab template](https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/files/gitlab-config-template/gitlab.rb.template)

#### 管理 GitLab
- `$ docker-compose pull`
- `$ docker-compose up -d` 启动，`-d` 表示后台运行，默认情况下会停止当前正在运行的容器，重新启动一个新容器
- `$ sudo tailf /srv/gitlab/logs/gitlab-rails/production.log` 宿主机查看日志

#### GitLab 命令
下面命令都需要进入 docker 容器执行
- `$ gitlab-ctl reconfigure` 重新加载配置
- `$ gitlab-ctl restart` 重启服务
- `$ gitlab-ctl tail` 监听日志


## 个性化设置
- 开启邮件服务，具体参数见上文配置文件
    - [GitLab邮件配置](https://www.jianshu.com/p/b91d2e676cba)
    - [云主机 TCP 25 端口出方向被封禁？](https://cloud.tencent.com/document/product/215/12390) 需要在腾讯云控制台申请解封 25 端口出口，否则邮件无法发送出来
- 关闭了注册功能，只能由管理员添加新用户，方便管理用户
    - [Git学习-->GitLab如何屏蔽掉注册功能？](https://blog.csdn.net/ouyang_peng/article/details/78562125)
- 设置自动备份，具体查看 [备份和还原](#备份和还原)


## 配置 Nginx 反向代理
通过 Nginx 实现域名到内部端口的映射，后期可在 Nginx 层面支持 HTTPS

添加下面的配置文件 `/etc/nginx/conf.d/gitlab.conf`，然后执行 `nginx -s reload` 命令加载配置文件：
```
server {
    server_name gitlab.example.com www.gitlab.example.com;
    listen 80;
    location / {
        proxy_pass_header Server;
        proxy_redirect off;

        # 设置最大允许上传单个的文件大小
        client_max_body_size 1024m;

        # 以下确保 gitlab 中项目的 url 是域名而不是 http://127.0.0.1:port，不可缺少
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        proxy_pass http://127.0.0.1:port;
    }
}
```


## 备份和还原
- [Backing up and restoring GitLab](https://docs.gitlab.com/ee/raketasks/backup_restore.html)
- [使用Gitlab一键安装包后的日常备份恢复与迁移](https://segmentfault.com/a/1190000002439923)

可以执行下面命令手动备份和恢复：

- `docker exec -it <name of container> gitlab-rake gitlab:backup:create`
- `docker exec -it <name of container> gitlab-rake gitlab:backup:restore`

也可以配置 `crontab` 定时任务自动备份：

- `sudo su -`
- `crontab -e`
- 在文件末尾加入 `0 2 * * * docker exec -t docker-gitlab-web gitlab-rake gitlab:backup:create`，表示每天夜里两点执行备份

定期生成的备份文件依旧在服务器上，需要手动从服务器拉取文件到本地，后期可考虑通过自动化脚本备份到指定机器；PS：GitLab 支持上传到 AWS，国内只能看看了-^-

`scp -i ssh_private_key -r username@ip:/srv/gitlab/backups/ .`


## References
- [nginx反向代理gitlab](https://www.jianshu.com/p/a86a1529d253)
- [使用docker部署gitlab应用](https://www.jianshu.com/p/05e3bb375f64)
- [RHEL7/CentOS7在线和离线安装GitLab配置使用实践](https://wsgzao.github.io/post/gitlab/)
- [Gitlab配置邮件服务【第三步】](https://iluoy.com/articles/17)
- [Gitlab之邮箱配置-yellowocng](https://blog.csdn.net/yelllowcong/article/details/79939589)


