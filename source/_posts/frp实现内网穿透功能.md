---
title: frp实现内网穿透功能
date: 2020-04-26 12:12:57
categories: 工具
---

# frp实现内网穿透功能

### 前期准备

  1. 一台公网主机（我是用的阿里云服务器，系统centOS 7）
  2. 内网主机（我用的是个人电脑，系统win10） 
  3. 域名
  4. 域名解析到公网主机

### 安装frp

 1. linux

     - 下载安装包，地址[https://github.com/fatedier/frp/releases](https://github.com/fatedier/frp/releases "下载地址")，选择对应的linux版本，上传至Linux服务器。或者选择下面方式下载：

       ```shell
       wget https://github.com/fatedier/frp/releases/download/v0.32.0/frp_0.32.0_linux_amd64.tar.gz
       ```

     - 解压 ``` tar -zxvf frp_0.32.0_linux_amd64.tar.gz```

     - 进入解压目录 ```cd frp_0.32.0_linux_amd64```，这里主要关注4个文件，分别是**frpc、frpc.ini** 和 **frps、frps.ini**，frpc是客户端需要用的文件，frps是服务端需要的文件


2. windows

    - 下载安装包，地址[https://github.com/fatedier/frp/releases](https://github.com/fatedier/frp/releases "下载地址")，选择对应的win版本，解压，进入解压目录。
    
    

3. 服务端（公网主机）配置：

   - 删除frpc、frpc.ini两个文件，然后编辑frps/ini文件 ```vi ./frps.ini```

     ```shell
     [common]
     bind_port = 7000           #与客户端绑定的进行通信的端口
     vhost_http_port = 8000     #访问客户端web服务自定义的端口号
     ```

       -  保存后启动服务 ``` ./frps -c ./frps.ini```，这个命令是前台启动可以使用nohup命令后台启动 ```nohup ./frps -c ./frps.ini &```
       -  在防火墙上开放刚才设置的两个端口

4. 客户端配置：

   - 删除frps、frps.ini两个文件，然后编辑frpc.ini

     ```shell
     [common]
     server_addr = 101.132.236.xxx   #公网服务器ip
     server_port = 7000            #与服务端bind_port一致
      
     #公网访问内部web服务器以http方式
     [web]
     type = http         #访问协议
     local_port = 12345   #内网web服务的端口号
     custom_domains = www.moyuu.fun   #所绑定的公网服务器域名
     ```

   - 如果是win系统，在当前目录中打开powerShell，执行``` ./frpc.exe -c ./frpc.ini```
   - 如果是linux则执行 ```nohup ./frpc -c ./frpc.ini &```

###  效果

1. 先访问本地web站点

   ![image-20200402132605338](http://file.moyuu.fun:10031/images/image-20200402132605338.png)

2. 通过域名访问（这里加了端口号，如果想要不加端口号就能访问，则需要在nginx里添加代理转发）

   ![image-20200402132638843](http://file.moyuu.fun:10031/images/image-20200402132638843.png)