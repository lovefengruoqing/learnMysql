## mysql的安装与测试

<div align="right"><i>时间：2018-01-12 13:44</i></div>

### mysql的安装验证

```
//我使用的是Win 7系统，root为我系统登录的账户，下面是在命令行键入查看mysql是否安装成功的命令，当然前提是你必须将mysql安装的文件夹的bin文件夹加入系统路径，否则你输入这个命令会提示不存在这个命令
C:\Users\root> mysqladmin --version
mysqladmin Ver 8.42 Distrib 5.7.20 for Win64 on x86_64
```

### mysql服务的启动

可以通过`services.msc`命令，在运行框中打开系统服务，然后找到mysql服务手动启动，或者用命令行等方式，但是我找了很多方法，貌似Windows上面采用这种方式是最直接的，其他试了好几种都没效果。

### mysql数据文件

因为Windows上面装mysql比较傻瓜式，不像unix或者Linux上面那么复杂，可能你在装完以后就无意之中设置了mysql数据文件存放的位置以及设置了用户名密码了，但是你却不知道。

我就对着官方的文档倒腾了半天，跟着他手动建立一个`my.ini`文件，然后，初始化数据文件夹，敲了一堆命令，然后发现一直在报错，一些奇怪的提示。看到后面才发现，原来我早就在安装数据库的时候就已经建立好了mysql数据文件存放的位置，也早已设置了用户名密码了，根本就不需要重复做这些动作。

如果你只是安装了数据库服务器，可以采取以下步骤来初始化mysql数据库文件存放位置

```c
1. On Windows, suppose that C:\my.ini contains these lines:
Initializing the Data Directory
//以下为my.ini文件内容
[mysqld]
basedir=C:\\Program Files\\MySQL\\MySQL Server 5.7  //数据库服务器安装的文件夹
datadir=D:\\MySQLdata   //数据库文件存放的位置，会自动创建，不需要你手动去创建

2. Then invoke mysqld as follows (the --defaults-file option must be first):
C:\> bin/mysqld --defaults-file=C:\my.ini --initialize  //如果你没有创建过数据库文件存放位置，那么这个命令会在你的d盘建立一个MySQLdata文件夹，作为保存数据库文件的位置，同时会为你随机生成一个密码，你必须得记下来

//如果你不想随机生成密码，想之后再手动设置，采用如下命令
C:\> bin/mysqld --defaults-file=C:\my.ini --initialize-insecure
```

### 连接到数据库服务器

```c
Connect to the server:
//• If you used --initialize but not --initialize-insecure to initialize the data directory,connect to the server as root using the random password that the server generated during the initialization sequence:
shell> mysql -u root -p     //如果你生成了随机密码，在命令行窗口键入的命令
Enter password: (enter the random root password here)

//Look in the server error log if you do not know this password.（如果你不知道密码，看一下err log文件，里面有记录）

//• If you used --initialize-insecure to initialize the data directory, connect to the server as root
without a password:
shell> mysql -u root --skip-password    //如果你忽略了密码，在命令行窗口键入的命令
```

### 更改账户密码

>After connecting, assign a new root password:
```c
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```

### 后记

当然，如果你不熟悉命令行的操作方式的话，以上过程，还可以采用客户端来完成，具体请参考[mysql官方文档](https://dev.mysql.com/doc/ "点击前往")，内容很丰富，你想知道的几乎都能找到答案，但是可能对于英语阅读有障碍的童鞋来说看的会很崩溃的。
