# Linux 常用命令 
更新时间: 2018.06.05

目录
---

<!-- TOC depthFrom:2 updateOnSave:true -->

- [端口测试](#端口测试)
- [账户相关](#账户相关)
- [密钥相关](#密钥相关)
- [网络相关](#网络相关)

<!-- /TOC -->

## 端口测试

```sh
nc -vz 10.130.1.42 80
```

## 账户相关

+ 创建用户

  ```shell
  adduser user1
  ```

+ 设置用户道密码

+ 把用户添加到用户组

+ 删除用户

+ 创建用户组

+ 删除用户组

## 密钥相关

+ 生成密钥
```shell
ssh-keygen -t rsa -C 'brandon_leaf@outlook.com'
cat ~/.ssh/id_rsa.pub   #公钥
cat ~/.ssh/id_rsa       #私钥
```

## 网络相关

+ 修改hosts
```shell
sudo gedit /etc/hosts
```
+ 重启网络
```shell
sudo /etc/init.d/networking restart
```