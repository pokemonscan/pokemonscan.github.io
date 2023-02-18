
##  服务端部署

### 环境准备

系统：`Linux`

软件：`docker` `docker-compose`


### 安装

下载安装包 `server.tar.gz`

```shell
tar -zxvf server.tar.gz
```

解压出三个文件

- docker-compose.yml
- install.sh
- openssl.cnf

执行安装脚本
```shell
bash install.sh
```

观察安装脚本输出，最后会输出日志

```log
success init admin user : password xxxxxxxxxx
```

此时新建了默认账户 admin ，密码见上述。
若忘记了密码，读取config目录下`daycare_config.yml`的`secret` 字段

访问 `http://ip:2314` 如果能正常访问，即安装成功

##  worker安装

### 安装 docker 

>略

### 从服务端获取 worker 安装命令 

点击`安装 worker`按钮，
复制安装命令，在 worker 上执行
```shell
curl http://ip:2314/pokemon/go?token=xxxxxx | bash
```
观察在 worker 列表是否有新 worker 上线