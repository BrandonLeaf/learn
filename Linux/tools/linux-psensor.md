# linux 温度检测工具
更新时间:2016.06.15

## 参考文章

* [如何在Linux系统上监测系统温度？](http://os.51cto.com/art/201311/417208.htm)

## 安装

* 安装psensor

    ```sh
    sudo apt-get install lm-sensors hddtemp psensor 
    ```

* 配置lm_sensors

    一路yes
    ```sh
    sudo sensors-detect 
    ```    

* 启动模块

    ```sh
    sudo service module-init-tools start
    ```

* 硬盘温度检测

    ```sh
    sudo hddtemp -d /dev/sda 
    ```