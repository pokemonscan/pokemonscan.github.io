
##  服务端部署

### 环境准备

系统：`Linux`

软件：`docker` `docker-compose`


### 通过 git clone 安装


#### 拉取仓库

git clone 仓库到本地

```shell
git clone https://github.com/taropowder/pokemon.git
```

#### 创建证书

```shell
bash script/server_init.sh
# Common Name (eg, fully qualified host name) []:pokemon.go
```


#### 启动服务
```shell
docker-compose up -d
```

访问 `http://127.0.0.1:2314` 如果能正常访问，即证明按照成功



##  worker安装

### 安装 docker 

>略

### 从服务端获取 worker 安装命令 

执行命令
```shell
curl http://ip:2314/pokemon/go | bash
```