# Docker 代理设置
更新时间: 2018.06.07

+ 创建配置文件
```sh
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo vi /etc/systemd/system/docker.service.d/http-proxy.conf
```
+ 添加内容如下

```sh
[Service]
Environment="HTTP_PROXY=http://[proxy-addr]:[proxy-port]/"
Environment="HTTPS_PROXY=https://[proxy-addr]:[proxy-port]/"
Environment="NO_PROXY=localhost,127.0.0.1,docker-registry.somecorporation.com"
```

> [proxy-addr] 代理地址  
> [proxy-port] 端口

+ 更新并重启

```sh
sudo systemctl daemon-reload
sudo systemctl restart docker
```