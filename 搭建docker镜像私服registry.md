### 搭建docker镜像私服registry

#### 1、安装register

准备安装环境

```shell
cd /usr/local
mkdir docker
cd docker 
mkdir registry
cd registry
vi docker-compose.yml
```

**docker-compose.yml**

```yml
version: '3.1'
services:
  registry:
    image: registry
    restart: always
    container_name: registry
    ports:
      - 5000:5000
    volumes:
      - /usr/local/docker/registry/data:/var/lib/registry
```

**测试**

浏览器访问 http://192.168.48.134:5000/v2/

#### 2、配置registry客户端

在`/etc/docker/daemon.json`增加如下内容

```json
{
  "registry-mirrors": [
    "https://registry.docker-cn.com"
  ],
  "insecure-registries": [
    "ip:5000"
  ]
}
```

之后重启服务

```shell
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

检查配置是否生效

```shell
docker info
```

使用 `docker info` 命令手动检查，如果从配置中看到如下内容，说明配置成功（192.168.75.134 为教学案例 IP）

```
Insecure Registries:
 192.168.75.134:5000
 127.0.0.0/8
```

测试镜像上传

```shell
## 拉取一个镜像
docker pull nginx

## 查看全部镜像
docker images

## 标记本地镜像并指向目标仓库（ip:port/image_name:tag，该格式为标记版本号）
docker tag nginx 192.168.75.134:5000/nginx

## 提交镜像到仓库
docker push 192.168.75.134:5000/nginx
```

查看全部镜像

```shell
curl -XGET http://192.168.75.134:5000/v2/_catalog
```

查看指定镜像

```shell
curl -XGET http://192.168.75.134:5000/v2/nginx/tags/list
```

测试拉取镜像

- 先删除镜像

```shell
docker rmi nginx
docker rmi 192.168.75.134:5000/nginx
```

- 再拉取镜像

```shell
docker pull 192.168.75.134:5000/nginx
```

#### 3、部署docker registry ui

这里介绍两个Docker Registry WebUI工具

- [docker-registry-frontend](https://github.com/kwk/docker-registry-frontend)
- [docker-registry-web](https://hub.docker.com/r/hyper/docker-registry-web/)

这里介绍docker-registry-frontend，使用docker-compose进行安装和运行，docker-compose.yml配置如下

```yml
version: '3.1'
services:
  frontend:
    image: konradkleine/docker-registry-frontend:v2
    ports:
      - 8080:80
    volumes:
      - ./certs/frontend.crt:/etc/apache2/server.crt:ro
      - ./certs/frontend.key:/etc/apache2/server.key:ro
    environment:
      - ENV_DOCKER_REGISTRY_HOST=192.168.75.134
      - ENV_DOCKER_REGISTRY_PORT=5000
```

安装并运行成功后在浏览器访问：http://192.168.48.134:8080

