# 常见问题

该文档收集了一些超算使用中的常见问题及对应解决方案。

#### yum执行的时候提示版本信息不全或者不对

错误信息: 

```bash
There was a problem importing one of the Python modules
required to run yum. The error leading to this problem was:

   /lib64/libxml2.so.2: symbol gzopen64, version ZLIB_1.2.3.3 not defined in file libz.so.1 with link time reference
```

解决方案：

添加环境变量。在`/etc/profile.d/`中新建一个`python.sh`文件，并在其中写入`export LD_LIBRARY_PATH=/lib64/:$LD_LIBRARY_PATH`。之后退出重进即可。

#### yum使用技巧

* yum本地离线安装文件：`yum localinstall xxx.rpm`
* yum往指定目录安装文件: `yum –c /etc/yum.conf –installroot=/your/install/path --releasever install your_file_installed`
* yum跨节点安装: `psh nodename “yum install xxx”`

#### 服务器文件管理技巧

* 批量删除进程。`ps –ef | grep aaa | grep –v xxx | cat –c 9-15 | xargs kill -9`. 其中aaa为要查找的该用户的进程，xxx是不需要删除的aaa用户进程。
* 跨节点传文件。`scp –rf file nodename:/the/path/`
* 使用root给所有用户添加环境变量。`/etc/profile.d/`文件夹中新建xxx.sh即可
* 发登陆界面通知。修改`/etc/motd`文件即可。建议修改后执行`cat motd >> motd-bak`,将通知备份保存到motd-bak文件中，以便做记录后期查看
* sinfo显示出的非正常节点状态。如果该节点状态为drained。则可通过scontrol update NodeName=xxx state=idle进行状态的刷新。刷新后可能恢复正常。如不正常则需查看该节点是否能够连接，是否非正常关机等。
* 添加额外的partition或者对其进行修改。可直接修改`/etc/slurm/slurm.conf`文件。具体参数可查看帮助。修改格式见该文件的最后几行。
* 登录时显示’abrt-cli status’ timeout。网上查了较多解决方式均不可用，最终通过`killall abrt-dbus`解决。

#### MySQL 相关问题

* 启动mysql。`systemctl start mysqld`。此命令不需要反复执行，只需要root在开机后执行一次即可。
* 设置mysql开机自启动。`systemctl enable mysqld`, `systemctl daemon-reload`
* 登录mysql。如果已经启动mysql则可直接登录，如未启动则需先启动。`mysql –u name –p password`.
* 更改mysql密码。密码可在登录近mysql之后更改。需要输入命令 `ALTER USER ‘username’ @ ‘locahost’ IDENTIFIED BY ‘new password’`。

#### 子节点网络连接和关闭

网线从顶部接口连接任一想要联网节点，目前只有root和g01插了网线
* 查看IP地址和网卡名 `ip addr show`
* 禁用某一网卡 `ifdown em1`
* 开启某一网卡并配置ip地址 `ifup em1`

Mysql用户root，密码”Passw0rd123.”
