# Code


## GitLab
- [GitLab](https://about.gitlab.com/)


#### 安装 GitLab
- [GitLab Docker images](https://docs.gitlab.com/omnibus/docker/)
- 采用 `docker-compose` 安装，具体配置见后面的 `docker-compose.yml`

`docker-compose.yml` 文件如下：
```
web:
  image: 'gitlab/gitlab-ce:latest'
  restart: always
  hostname: '域名'
  environment:
    GITLAB_OMNIBUS_CONFIG: |
      external_url 'http://域名'
      # Add any other gitlab.rb configuration here, each on its own line
  ports:
    - '20443:443'
    - '2080:80'
    - '2022:22'
  volumes:
    - '/srv/gitlab/config:/etc/gitlab'
    - '/srv/gitlab/logs:/var/log/gitlab'
    - '/srv/gitlab/data:/var/opt/gitlab'
```
其中 `GITLAB_OMNIBUS_CONFIG` 用于自定义 GitLab 默认配置，配置项见：[Omnibus GitLab template](https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/files/gitlab-config-template/gitlab.rb.template)


#### Nginx 配置
```
server {
    server_name 域名 www.域名;
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


## References
- [nginx反向代理gitlab](https://www.jianshu.com/p/a86a1529d253)
- [使用docker部署gitlab应用](https://www.jianshu.com/p/05e3bb375f64)


