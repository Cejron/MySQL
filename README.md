# MySQL

[安装](#安装)

[卸载](#卸载)

---

#### 安装

##### 下载zip

 [官网下载](https://dev.mysql.com/downloads/mysql/) 

 [国内镜像下载](http://mirrors.sohu.com/mysql/)  ---进去先ctrl+f检索mysql，选择合适的mysql版本，再搜x64(此处选择合适自己电脑的配置)

##### 解压
解压zip，文件夹移至`C:\Program Files`目录下，配置环境变量，添加`C:\Program Files\mysql-8.0.19-winx64\bin;`至`Path`

##### 以上两步过后分别有两种方法配置mysql。

`法一:`
1.以管理员身份进入cmd

2.安装musql服务
进入mysql的bin目录下，安装服务 `mysqld  -install`
若提示 `The service already exists!` 说明之前安装过
需要使用 `mysqld -remove MySQL` 命令先卸载它
卸载的原因是之前安装的版本可能跟现在安装的版本不一致，可能会产生冲突

3.启动mysql服务
在计算机管理中启动MySQL服务或者管理cmd中执行 `net start mysql`

4.配置初始化
`mysqld --initialize --console` 自动生成MySQL的初始化配置(data文件目录等)，会有一个初始密码要记住

这一步和安装并启动服务无必要先后顺序，也可以先配置得到初始密码再安装和启动服务

5.登陆mysql终端
`mysql -uroot -p` 需要通过初始化配置获取初始密码登陆

`mysql -uroot -p` 跟 `mysql -u root -p`效果上无区别

这里若是报错，说明没装服务并且没开服务或装了服务但是没开服务

7.使用 `show databases;` 测试下先，报错则说明需要使用一下命令重置密码
也可以直接设置密码,命令如下,二选一 :
`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码';` 或 `alter user root@localhost identified by '新密码';`(sql语句一般要以分号结尾) 

`法二:`
1.在mysql目录下，不用进入bin，我这里是 `C:\Program Files\mysql-8.0.19-winx64`目录下 创建 `my.ini` 文件,复制以下内容到`my.ini`

```
[mysqld]
# 主库和从库需要不一致
server-id=1
log-bin=mysql-bin
# 需要同步的数据库
#binlog-do-db=test
# 不需要同步的数据库
#binlog-ignore-db=mysql
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=C:\Program Files\mysql-8.0.19-winx64
# 设置mysql数据库的数据的存放目录
dtadir=C:\Program Files\mysql-8.0.19-winx64\Data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
#插件认证方式caching_sha2_password和mysql_native_password
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```

要注意的是这两个配置，要自己设置保存目录：
`basedir`
`datadir`

2.使用命令安装mysql服务
`mysqld --install MySQL --defaults-file="C:\Program Files\mysql-8.0.19-winx64\my.ini"` 其中 `MySQL` 是服务名

3.启动mysql服务`net start mysql`

4.初始化配置
`mysqld --initialize --user=mysql --console`
成功的话会生成随机密码，记下生成的密码

5.登陆mysql终端
`mysql -uroot -p`，然后输入上面生成的密码，进入mysql欢迎界面

6.修改root密码
`alter user root@localhost identified by '新密码';`(sql语句一般要以分号结尾)


##### mysql可视化
此时无法做到数据库可视化
为了可视化管理数据库，一般采用第三方软件，如Navicat Premium


##### 配置远程访问
1.使用命令`mysql -uroot -p` 因为已经修改过密码了，所以直接输入新密码，登陆到mysql终端中

2.切换到到名为mysql数据库: mysql>`use mysql;`

3.`update user set host = '%' where user = 'root';`设置为所有机器都能访问root

4.最后执行完刷新一下：`flush privileges;`




##### mysql终端常用命令
`mysql -uroot -p` 并输入密码进入终端
mysql>`show databases;`查看有什么数据库，能看到有个mysql
mysql>`use mysql;`进入mysql这个数据库
mysql>`show tables;`查看mysql这个数据库中的表，能看到有个user
mysql>`desc user;`查看user表结构


---

#### 卸载

1、先停止mysql服务，可通过命令行停止，输入 `net stop mysql;`
 还可以通过右键->计算机->管理->服务和应用程序->服务，找到MySQL，右键停止。

2、再卸载mysql服务，命令行输入`sc delete mysql` 或 `mysqld -remove MySQL`

3、删除与mysql相关的注册表，运行注册表：win+r，输入regedit，打开注册表。

删除
`HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\Eventlog\Application\MySQLD Service`文件夹

删除
`HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\Services\Eventlog\Application\MySQLD Service`文件夹

删除
`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Eventlog\Application\MySQLD Service`的文件夹

以上有可能一个或多个，都删掉即可，目录下没有就不用理会

4、命令行窗口输入`sc delete mysql` 提示删除成功！

5、清空安装mysql路径的文件夹即可。

[参考资料](https://www.cnblogs.com/rcg714786690/p/12371818.html)
