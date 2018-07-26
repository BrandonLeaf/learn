# Ubuntu 虚拟内存扩容
更新日期:2018.06.12

## 查看Swap分区大小

```sh
free -m
-------------------
16000000+0 records in
16000000+0 records out
16384000000 bytes (16 GB, 15 GiB) copied, 178.455 s, 91.8 MB/s
```

## 开始扩容

+ 创建Swap目录

    找个大目录
    ```sh
    sudo mkdir /swap
    cd /swap
    ```

+ 创建Swap文件

    count=<内存大小，如16G，即16000000>
    ```sh
    sudo dd if=/dev/zero of=swapfile bs=1024 count=16000000
    ---------执行后显示以下内容-----------------
    16000000+0 records in
    16000000+0 records out
    16384000000 bytes (16 GB, 15 GiB) copied, 178.455 s, 91.8 MB/s

    ```

+ 转换为swapfile

    ```sh
    sudo mkswap -f swapfile
    -----------执行后显示以下内容---------------
    mkswap: swapfile: insecure permissions 0644, 0600 suggested.
    Setting up swapspace version 1, size = 15.3 GiB (16383995904 bytes)
    no label, UUID=a92be5c7-60a8-47a6-9e44-ac37151e3a4a
    ```

+ 应用文件

    ```sh
    sudo swapon swapfile
    --------------执行后显示以下内容-----------
    swapon: /swap/swapfile: insecure permissions 0644, 0600 suggested.
    ```

+ 查看swap分区空间

    检查是否变大了
    ```sh
    free -m
    -------------执行后显示以下内容-----------
            total   used   free   shared  buff/cache   available
    Mem:    7901    4621   1934   27      1345         2974
    Swap:   16604   372    16232
    ```

+ 一直保持swap

    修改启动文件
    ```sh
    sudo vi /etc/fstab
    ---------添加以下内容------------------
    /swap/swapfile /swap swap defaults 0 0
    ```

+ 卸载swap

    ```sh
    sudo swapoff swapfile
    ```
    
    
