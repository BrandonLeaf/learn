# MySQL
更新时间: 2018.06.05

目录
---

<!-- TOC depthFrom:2 updateOnSave:true -->

- [1. 密码重置](#1-密码重置)

<!-- /TOC -->

## 1. 密码重置

+ 方法1

    ```sql
    SET PASSWORD FOR ‘root’@’localhost’ = PASSWORD('123');
    ```


+ 方法2

    ```sql 
    UPDATE user SET password=PASSWORD('123') WHERE user=’root’; 
    UPDATE user SET password=PASSWORD('123') 
    where USER=’root’; 
    ```

