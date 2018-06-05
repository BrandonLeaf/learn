# Linux 常用命令 

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