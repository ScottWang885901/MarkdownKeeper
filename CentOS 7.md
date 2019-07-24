



# CentOS 7

## 系统相关

| 命令            | 解析                                               |
| :-------------- | :------------------------------------------------- |
| su -            | 切换到root权限（在普通用户下，exit也可切换到root） |
| su - [用户名]   | 切换到该用户下（su - scott）                       |
| shutdown -h now | 关机                                               |
| shutdown -r now | 重启                                               |
| man ping        | 查看参考手册（例如ping 命令）                      |
| passwd          | 修改密码                                           |

## 文件相关

| 命令                    | 解析                                                         |
| :---------------------- | :----------------------------------------------------------- |
| cd /home                | 进入 ‘/home’ 目录                                            |
| cd ..                   | 返回上一级目录                                               |
| cd ../..                | 返回上两级目录                                               |
| cd -                    | 返回上次所在目录                                             |
| cp file1 file2          | 将file1复制为file2                                           |
| cp -a dir1 dir2         | 复制一个目录                                                 |
| cp -a /tmp/dir1 .       | 复制一个目录到当前工作目录（.代表当前目录）                  |
| ls                      | 查看目录中的文件                                             |
| ls -a                   | 显示隐藏文件                                                 |
| ls -l                   | 显示详细信息                                                 |
| ls -lrt                 | 按时间显示文件（l表示详细列表，r表示反向排序，t表示按时间排序） |
| pwd                     | 显示工作路径                                                 |
| mkdir dir1              | 创建 ‘dir1’ 目录                                             |
| mkdir dir1 dir2         | 同时创建两个目录                                             |
| mkdir -p /tmp/dir1/dir2 | 创建一个目录树                                               |
| mv dir1 dir2            | 移动/重命名一个目录                                          |
| rm -f file1             | 删除 ‘file1’                                                 |
| rm -rf dir1             | 删除 ‘dir1’ 目录及其子目录内容                               |

## Crontab

| 命令                  | 解析         |
| :-------------------- | :----------- |
| service crond start   | 启动服务     |
| service crond stop    | 关闭服务     |
| service crond restart | 重启服务     |
| service crond status  | 查看服务状态 |
| service crond reload  | 重新载入配置 |
|                       |              |

## 权限相关

## 项目部署

### centos第一次部署spring boot项目

- 下载安装jdk

- 打包springboot项目

  本地有一个springboot的helloworld 项目，先用maven打包，打开eclipse的terminal,在该项目下输入maven 打包命令  mvn package

  ![1563961038313](D:\Typora_Workspace\CentOS 7.assets\1563961038313.png)

​       打包成功后，拿到jar包用sftp 工具将其上传到linux 服务器

- 启动springboot项目
  找到上传的项目，利用

  ~~~ shell
  java -jar spring-boot-hello-0.0.1-SNAPSHOT.jar
  ~~~

  启动

- 启动后查看端口号和controller 的映射

  ![1563961069248](D:\Typora_Workspace\CentOS 7.assets\1563961069248.png)

- 通过ip地址+端口号访问，但是访问不到，但是服务器是可以ping 到的。

  ![1563961131810](D:\Typora_Workspace\CentOS 7.assets\1563961131810.png)

- 下面解决这个问题：

  ![1563961156624](D:\Typora_Workspace\CentOS 7.assets\1563961156624.png)

​       使用telnet 连接8080端口时显示失败，考虑是不是服务器防火墙的原因

~~~shell
firewall-cmd --state    #查看防火墙的状态
~~~

![1563961177748](D:\Typora_Workspace\CentOS 7.assets\1563961177748.png)

提示要用超级用户

~~~shell
su -                  #切换超级用户
~~~

再次使用查看防火墙状态的命令

![1563961193008](D:\Typora_Workspace\CentOS 7.assets\1563961193008.png)

可以看到防火墙running

- 关闭防火墙

  ~~~shell
  systemctl stop firewalld.service
  ~~~

- 再次telnet 8080 端口，成功！访问，成功！

  ![1563961208678](CentOS 7.assets\1563961208678.png)












## Vim



## jdk的安装

1. 准备工作：安装依赖环境

   
   
   

## 网络配置

### 关于Hyper-v虚拟机的网络配置

[详解Hyper-v虚拟机的网络配置](https://www.cnblogs.com/dfzone/archive/2013/04/26/3044177.html)

上面的文章提到：

- **External（外部虚拟网络）：**     <u>虚拟机和物理网络、本地主机都能通信</u>

在希望允许子分区（虚拟机或guest）与外部服务器和父分区（管理操作系统或host）进行通信时，可以使用此类型的虚拟网络。此类型的虚拟网络还允许位于同一物理服务器上的虚拟机互相通信。

所以只要为虚拟机创建一个外部虚拟网络，即可实现宿主机与虚拟机的互通，虚拟机与外网的互通。

下面是创建步骤：



1. 给Centos新建一个外部的虚拟交换机，选中右侧Virtual Switch Manager

![1563868448451](C:\Users\swang25\AppData\Roaming\Typora\typora-user-images\1563868448451.png)

2. 选中左侧New virtual network swith ------->右侧面板中 External---------->下方新建 Create virtual switch

![1563868561637](C:\Users\swang25\AppData\Roaming\Typora\typora-user-images\1563868561637.png)

3.  选中新建的虚拟交换机，可以自定义Name，Connection Type 处选择外部网络 External network；  我选择的是Intel(R) Ethernet Connection(2)选项,一般与你宿主机网络一致；默认勾选运行管理员分销适配器  Allow management operating system to share this network adapter。click  Apply-->OK;

![1563868988487](C:\Users\swang25\AppData\Roaming\Typora\typora-user-images\1563868988487.png)



现在你的网络适配器会多一个

![1563869232109](C:\Users\swang25\AppData\Roaming\Typora\typora-user-images\1563869232109.png)

4. 给你的centos 添加适配器，到你的虚拟机下面选择Settings

![1563874913594](C:\Users\swang25\AppData\Roaming\Typora\typora-user-images\1563874913594.png)



5. 添加网络适配器 Add Hardware ----->Network Adapter--------->ok

    ![1563866743775](C:\Users\swang25\AppData\Roaming\Typora\typora-user-images\1563866743775.png)

   

   6. 指定网络网络适配器，选择刚刚配置的外部适配器Centos virtual switch

      ![](C:\Users\swang25\AppData\Roaming\Typora\typora-user-images\1563868182694.png)

      

      ![1563875395202](C:\Users\swang25\AppData\Roaming\Typora\typora-user-images\1563875395202.png)

   

   7. 给虚拟机配置网卡,编辑下面的文件

      ~~~shell
      vim /etc/sysconfig/network-scripts/ifcfg-eth0
      ~~~

      关键两项：

      ~~~tex
      ONBOOT=YES  (开机启动)
      IPADDR=XX.XX.XX.XX (设置ip与你宿主机同一网段)
      ~~~

8. 重启机器验证即可

   - ping外网

   ![1563889550592](C:\Users\swang25\AppData\Roaming\Typora\typora-user-images\1563889550592.png)

   

   - 宿主机ping centos

![1563889640442](C:\Users\swang25\AppData\Roaming\Typora\typora-user-images\1563889640442.png)






















