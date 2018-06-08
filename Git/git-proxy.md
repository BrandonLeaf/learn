# Git 代理设置
更新时间:2018.06.07

目录
---

<!-- TOC depthFrom:2 updateOnSave:true -->

- [http/https 代理](#httphttps-代理)
- [SSH 代理](#ssh-代理)

<!-- /TOC -->


## http/https 代理

+ 添加代理

```sh
git config --global http.proxy 'http://127.0.0.1:1080'
git config --global https.proxy 'http://127.0.0.1:1080'
```

+ 删除代理

``` sh
git config --global --unset https.proxy
```

## SSH 代理

+ 安装 corkscrew

```sh
sudo apt install corkscrew
```

+ 修改 ~/.ssh/config 内容如下
```sh
Host github.com
User git
Hostname ssh.github.com
Port 443
ProxyCommand /usr/bin/corkscrew proxy_server proxy_port %h %p ~/.ssh/proxyauth
IdentityFile ~/.ssh/id_rsa
```
> proxy_server 为代理服务器地址，proxy_port 为代理服务器端口。

+ 修改 ~/.ssh/proxyauth

```sh
username:password
```

> 写上代理用户名密码,没有也要建空文件

+ 测试
```sh
ssh -T git@github.com
```

> 成功信息： You've successfully authenticated, but GitHub does not provide shell access.
