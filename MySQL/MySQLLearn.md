# MySQL Study

<!-- TOC depthFrom:2 -->

- [1. 密码重置](#1-密码重置)

<!-- /TOC -->

## 1. 密码重置

+ 方法1

    ```sql
    SET PASSWORD FOR ‘root’@’localhost’ = PASSWORD('Asdfqwe123');
    ```


+ 方法2

    ```sql 
    UPDATE user SET password=PASSWORD('Asdfqwe123') WHERE user=’root’; 
    UPDATE user SET password=PASSWORD('Asdfqwe123') 
    where USER=’root’; 
    ```

