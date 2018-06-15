# Docker Command


## 常用命令
- `$ docker info`
- `$ docker version`
- `$ docker-compose --version`
- `$ docker-machine --version`
- `$ docker system df` 查看镜像、容器、数据卷所占用的空间
- `$ docker commit` 将修改之后的容器保存为镜像，建议不用

#### Image
- `$ docker image ls` 列出所有顶级镜像
- `$ docker image ls ubuntu`
- `$ docker image ls buntu:16.04`
- `$ docker image ls --digests` 显示镜像摘要
- `$ docker image rm [选项] <镜像1> [<镜像2> ...]` 删除镜像，`<镜像>` 可以是 镜像短 ID、镜像长 ID、镜像名（<仓库名>:<标签>） 或者 镜像摘要
- `$ docker image ls -f dangling=true` 查看虚悬镜像(dangling image)
- `$ docker image prune` 删除虚悬镜像
- `$ docker image rm $(docker image ls -q redis)` Untagged 和 Deleted

#### Container
- `$ docker stop webserver`
- `$ docker rm webserver`


## Example
#### 交互式启动，退出时删除容器
```
$ docker run -it --rm \
    ubuntu:16.04 \
    bash
```

- `-it`：这是两个参数，一个是 -i：交互式操作，一个是 -t 终端。我们这里打算进入 bash 执行一些命令并查看返回结果，因此我们需要交互式终端。
- `--rm`：这个参数是说容器退出后随之将其删除。默认情况下，为了排障需求，退出的容器并不会立即删除，除非手动 docker rm。

#### 指定容器名称，并映射端口号
```
$ docker run --name web2 -d -p 81:80 nginx:v2
```

















