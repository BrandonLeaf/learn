# MySQL 主从同步
更新时间:2018.06.09

目录
---
<!-- TOC depthFrom:2 updateOnSave:true -->

- [相关语句](#相关语句)
- [用例环境说明](#用例环境说明)
- [步骤](#步骤)

<!-- /TOC -->

## 相关语句
+ 查看主库状态
    ```sql
    show master status;
    ```
+ 查看从库状态
    ```sql
    show slave status;
    ```
+ 查询只读状态
    ```sql
    show global variables like "%read_only%";
    ```
+ 设置只读状态
    ```sql
    #取消只读状态
    set global read_only=0;
    #设置只读状态
    set global read_only=1;
    ```

+ 建立主从联系
    ```sql
    --在从机中执行
    change master to 
    master_host='192.168.1.10',--主机IP
    master_port=3306,--主机mysql端口
    master_user='slave',--主机建立的用户
    master_password='123123',
    master_log_file='bin.000140',--从主机中show master status获取
    master_log_pos=679831629;--从主机中show master status获取
    ```

## 用例环境说明

+ Linux
+ 版本：MySQL 5.7
+ A机器为主,IP: 192.168.1.10
+ B机器为从,IP：192.168.1.11

## 步骤

1. 主库建立主从连接的用户(建用户>配角色)
    + 方法一
        1. 使用Workbanch
        2. 进入主数据库
        3. Management > Users and Privileges
        4. 创建用户
        5. 进入用户角色配置 > Administrative Roles
        6. 只勾选Custom
        7. 在右边 global Privileges 中勾选 REPLICATION SLAVE
        
2. 主库设置只读
    ```sql
    set global read_only=0;
    ```
1. 从库建立联系
    ```sql
    --在从机中执行
    change master to 
    master_host='192.168.1.10',--主机IP
    master_port=3306,--主机mysql端口
    master_user='slave',--主机建立的用户
    master_password='123123',
    master_log_file='bin.000140',--主机中show master status获取
    master_log_pos=679831629;--主机中show master status获取
    ```
2. 从库查询同步情况
    ```sql
    show slave status;
    --查看Slave_IO_State状态是否有异常信息
    --查看master_log_file是否与主库show master status中的一样
    --查看read_master_log_pos是否与主库show master status中的一样
    ```
3. 主库取消只读
    ```sql
    set global read_only=1;
    ```