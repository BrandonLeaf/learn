# MySQL 主从同步
更新时间:2018.06.11

目录
---
<!-- TOC depthFrom:2 updateOnSave:true -->

- [相关语句](#相关语句)
- [用例环境说明](#用例环境说明)
- [空库同步步骤](#空库同步步骤)
- [主从数据同步](#主从数据同步)
    - [方法一(快速，忽略差异数据)](#方法一快速忽略差异数据)
    - [方法二(重新做主从，完全同步，速度因数据多而慢)](#方法二重新做主从完全同步速度因数据多而慢)
- [常见问题](#常见问题)

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
+ 只读状态

    ```sql
    --取消只读状态
    set global read_only=0;
    --设置只读状态
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
+ 锁表查询

    ```sql
    show status like '%lock%';
    ```

+ 锁表

    ```sql
    flush tables with read lock; 
    ```

+ 解锁
    ```sql
    UNLOCK TABLES;
    ```

+ 查询异常原因

    ```sql
    select * from performance_schema.replication_applier_status_by_worker;
    ```

## 用例环境说明

+ Linux
+ 版本：MySQL 5.7
+ A机器为主,IP: 192.168.1.10
+ B机器为从,IP：192.168.1.11





## 空库同步步骤

1. 主库建立主从连接的用户(建用户>配角色)

    + 方法一
        1. 使用Workbanch
        2. 进入主数据库
        3. Management > Users and Privileges
        4. 创建用户
        5. 进入用户角色配置 > Administrative Roles
        6. 只勾选Custom
        7. 在右边 global Privileges 中勾选 REPLICATION SLAVE
        
1. 主库锁表

    ```sql
    flush tables with read lock; 
    ```
    
3. 从库建立联系

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
    
3. 从库查询同步情况

    ```sql
    show slave status;
    --查看Slave_IO_State状态是否有异常信息
    --查看master_log_file是否与主库show master status中的一样
    --查看read_master_log_pos是否与主库show master status中的一样
    ```
    
4. 主库解锁

    ```sql
    UNLOCK TABLES;
    ```

## 主从数据同步

### 方法一(快速，忽略差异数据)

+ 应用环境

    适用于主从库数据相差不大，或者要求数据可以不完全统一的情况，数据要求不严格的情况 

+ 步骤
    1. 从库停止同步

        ```sql
        stop slave;
        ```
    1. 表示跳过一步错误，后面的数字可变
    
        ```sql
        set global sql_slave_skip_counter =1;
        ```
        
    1. 从库启动同步

        ```sql
        start slave;
        ```
    
    1. 从库检查同步状态

        ```sql
        show slave status;
        ```
        
        显示如下即同步成功
        
        ```
        Slave_IO_Running: Yes 
        Slave_SQL_Running: Yes 
        ```
        
    1. 主库解锁

        ```sql
        UNLOCK TABLES;
        ```

### 方法二(重新做主从，完全同步，速度因数据多而慢)

+ 应用环境

    适用于主从库数据相差较大，或者要求数据完全统一的情况

+ 步骤

    1. 主库锁表，设置只读

        ```sql
        --进入MySQL客户端
        flush tables with read lock; 
        set global read_only=1;
        ```
        
    1. 主库备份数据

        ```sh
        --退出MySQL客户端
        mysqldump -uroot -p --all-databases > mysql.bak.sql 
        
        ```
        
        ps:找个大点的分区导
        
    1. 主库传送数据到从库

        ```sh
        scp mysql.bak.sql mysql@192.168.1.11:/mysqlData/dbtmep
        ```
        
    1. 从库停止同步状态

        ```sql
        stop slave;
        ```

    1. 从库导入备份数据

        ```sql
        source /mysqldata/dbTemp/mysql.bak.sql
        ```
        
    1. 从库设置同步

        + 需要重设slave

            ```sql
            reset slave;
            ```
        + 从库建立联系

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
        + 开启同步

            ```sql
            start slave;
            ```
            
    7. 从库查询同步状态

         ```sql
        show slave status;
        ```
        
        显示如下即同步成功
        
        ```
        Slave_IO_Running: Yes 
        Slave_SQL_Running: Yes 
        ```
    
    1. 主库解锁,解只读

        ```sql
        UNLOCK TABLES;
        set global read_only=0;
        ```

## 常见问题

+ 问题
    ```
    Coordinator stopped because there were error(s) in the worker(s). The most recent failure being: Worker 1 failed executing transaction '213a64cd-904e-11e7-b5e0-0664da000bc0:90431118' at master log bin.000140, end_log_pos 707965683. See error log and/or performance_schema.replication_applier_status_by_worker table for more details about this failure or others, if any.
    ```
+ 查询问题

    执行以下语句
    ```sql
    select * from performance_schema.replication_applier_status_by_worker;
    ```
    显示如下
    ```
    Worker 1 failed executing transaction '213a64cd-904e-11e7-b5e0-0664da000bc0:90431118' at master log bin.000140, end_log_pos 707965683; Could not execute Update_rows event on table db_dg_wx.t_cust_reg; Can't find record in 't_cust_reg', Error_code: 1032; handler error HA_ERR_KEY_NOT_FOUND; the event's master log bin.000140, end_log_pos 707965683
    ```